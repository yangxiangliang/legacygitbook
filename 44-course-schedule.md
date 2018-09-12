# 207. Course Schedule

> There are a total ofncourses you have to take, labeled from `0`to `n - 1`.
>
> Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`
>
> Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?
>
> ** Note:**
>
> You may assume that there are no duplicate edges in the input prerequisites.

### 思路

这里每个课程就是vertice，然后prerequisite就是一个edge，就是detect 该directed graph有没有cycle。因为没有cycle的话，就可以用topological sort，就可以得出一个方案来finish all courses；如果有cycle，则没办法finish所有课程。

### Code 1\) DFS

这里是用DFS，用一个boolean\[\] visited来记录所有visit过的点，一个boolean\[\] onpath来记录当前DFS visit过的点，如果当前DFS path上visit到了重复的点，则说明有cycle。

```java
public class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        boolean[] visited = new boolean[numCourses];
        boolean[] onpath = new boolean[numCourses];
        HashMap<Integer, List<Integer>> graph = new HashMap();
        // build graph
        for(int[] edge : prerequisites) {
            if(!graph.containsKey(edge[0])) graph.put(edge[0], new ArrayList());
            if(!graph.containsKey(edge[1])) graph.put(edge[1], new ArrayList());
            graph.get(edge[1]).add(edge[0]);
        }
        for(int i = 0; i < numCourses; i ++) {
            if(!visited[i] && hasCycle(graph, visited, onpath, i)) return false;
        }
        return true;
    }

    public boolean hasCycle(HashMap<Integer, List<Integer>> graph, boolean[] visited, boolean[] onpath, int cur) {
        if(!graph.containsKey(cur)) return false;
        if(visited[cur]) return false;
        onpath[cur] = true;
        visited[cur] = true;
        for(int neighbor : graph.get(cur)) {
            if (onpath[neighbor] || hasCycle(graph, visited, onpath, neighbor)) return true;
        }
        // reset bool[] onpath的current node 为FALSE
        onpath[cur] = false;
        return false;
    }
}
```

### Code 2\) BFS

这里用的是Topological Sorting中的BFS方法，即Kahn's algorithm。由于这里不用返回最后的topological sort的结果，只用判断有没有cycle，所以只用记录过程remove掉的edge的数量，如果等于总的edge数量，说明没有cycle；反之则有cycle。

```java
public class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        boolean[] visited = new boolean[numCourses];
        boolean[] onpath = new boolean[numCourses];
        HashMap<Integer, List<Integer>> graph = new HashMap();
        // build graph
        int[] inDegrees = new int[numCourses];
        for(int[] edge : prerequisites) {
            if(!graph.containsKey(edge[0])) graph.put(edge[0], new ArrayList());
            if(!graph.containsKey(edge[1])) graph.put(edge[1], new ArrayList());
            graph.get(edge[1]).add(edge[0]);
            inDegrees[edge[0]] ++;
        }
        // 把所有没有incoming edges的vertice加入queue中
        Queue<Integer> queue = new LinkedList();
        for (int i = 0; i < numCourses; i ++) {
            if (inDegrees[i] == 0) queue.add(i);
        }

        int removedEdges = 0;
        while(!queue.isEmpty()) {
            int cur = queue.poll();
            if (!graph.containsKey(cur)) continue;
            for(int neighbor : graph.get(cur)) {
                // remove 掉neighbor中的incoming edge，如果该neighbor的incoming edge变为0，则加入queue中
                inDegrees[neighbor] --;
                removedEdges ++;
                if (inDegrees[neighbor] == 0) queue.add(neighbor);
            }
        }
        return removedEdges == prerequisites.length;
    }
}
```



