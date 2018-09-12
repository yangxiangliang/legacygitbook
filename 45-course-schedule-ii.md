# 210. Course Schedule II

> There are a total ofncourses you have to take, labeled from `0`to`n - 1`.
>
> Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair:`[0,1]`
>
> Given the total number of courses and a list of prerequisite **pairs**, return the ordering of courses you should take to finish all courses.
>
> There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.
>
> **Note:** You may assume that there are no duplicate edges in the input prerequisites.

### 思路

就是典型的topological sort，但是根据题目，不知道其中有没有cycle，解题时需要注意这点。常用的Topological Sort有两种方法，BFS, DFS都有。

#### **1）BFS**

先找到所有没有incoming degrees的vertice，将其加入queue中。从queue中poll出current vertice，将current vertice加入结果中，遍历current vertice的所有neighbor，将每个neighbor的incoming degrees的个数减1，如果neighbor也没有了incoming degrees，那么将该neighbor加入queue中。这样直到queue变成empty。

**Detect Cycle**: 如果该graph中cycle，那么removed 掉的edge肯定不等于总的edges，总有vertice没有加入到queue和最后结果中，所以结果中加入的元素个数也小于总的vertice个数。

```java
public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        HashMap<Integer, List<Integer>> graph = new HashMap();
        // 记录vertice有多少个incoming degrees
        int[] inDegrees = new int[numCourses];
        // build graph
        for(int[] edge : prerequisites) {
            if(!graph.containsKey(edge[0])) graph.put(edge[0], new ArrayList());
            if(!graph.containsKey(edge[1])) graph.put(edge[1], new ArrayList());
            graph.get(edge[1]).add(edge[0]);
            inDegrees[edge[0]] ++;
        }
        Queue<Integer> queue = new LinkedList();
        for (int i = 0; i < numCourses; i ++) {
            if (inDegrees[i] == 0) queue.add(i);
        }
        
        int[] rst = new int[numCourses];
        int index = 0;
        while(!queue.isEmpty()) {
            int cur = queue.poll();
            rst[index++] = cur;
            if (!graph.containsKey(cur)) continue;
            for(int neighbor : graph.get(cur)) {
                inDegrees[neighbor] --;
                if (inDegrees[neighbor] == 0) queue.add(neighbor);
            }
        }
        if (index != numCourses) return new int[0];
        return rst;
    }
}
```



#### **2\)  DFS**

这里的思路是对每一个vertice进行DFS，对vertice的所有neighbor也DFS，neighbor的neighbor也进行DFS....当然过程中记录哪些visit过了，哪些没有visit过。由于DFS是遍历到最"深"的vertice才返回，如果在DFS function中将vertice加入result中，最后的结果是按照Topological Sort来的最后面的vertice在result中的最靠前，是reverse的。所以在DFS过程中，我们将vertice 压入stack中，最后一个一个pop出来加入result中才是正确的顺序。当然，需要将current vertice的所有neighbor DFS遍历完后，即将其所有neighbor 压入stack中后，才push current vertice 入Stack，这样才能保证结果中的拓扑排序。

** Detect Cycle: ** 用DFS 找directed graph的cycle时知道，如果在DFS current vertice过程中又回到了该DFS path上的某个到过的vertice时，便说明有cycle。所以这里不用boolean\[\] visited 来记录某个vertice是否visit过，用int\[\] visited来记录，0表示unvisited，1表示visiting\(即当前正在visit\)， 2表示visited\(即current vertice以及其neighbor都被DFS结束了\)。

```java
public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] visited = new int[numCourses]; // 0 -> unvisited, 1 -> visiting, 2 -> visited
        HashMap<Integer, List<Integer>> graph = new HashMap();
        // build graph
        for(int[] edge : prerequisites) {
            if(!graph.containsKey(edge[0])) graph.put(edge[0], new ArrayList());
            if(!graph.containsKey(edge[1])) graph.put(edge[1], new ArrayList());
            graph.get(edge[1]).add(edge[0]);
        }
        
        Stack<Integer> stack = new Stack();
        for(int i = 0; i < numCourses; i++) {
            if(visited[i] == 0) {
                if (topoSort(graph, stack, visited, i) == false) return new int[0];
            }
        }
        int[] rst = new int[numCourses];
        for(int i = 0; i < numCourses; i ++) rst[i] = stack.pop();
        return rst;
    }
    
    // false 表示该graph有cycle
    public boolean topoSort (HashMap<Integer, List<Integer>> graph, Stack<Integer> stack, int[] visited, int cur) {
        if(visited[cur] == 2) return true;
        // visit回到了该DFS path上正在visit的vertice，即发现cycle
        if(visited[cur] == 1) return false;
        visited[cur] = 1; // starting DFS current vertice
        if(graph.containsKey(cur)) {
            for(int neighbor : graph.get(cur)) {
                if(topoSort(graph, stack, visited, neighbor) == false) return false;
            }
        }
        // finish DFS current vertice
        visited[cur] = 2;
        stack.push(cur);
        return true;
    }
}
```



