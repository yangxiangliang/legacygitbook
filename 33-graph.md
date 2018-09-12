# Graph

1. Detect **cycle** in **undirected** graph：用union-find来解决，遍历graph中所有edges，如果某个edge的2个vertices都属于同一个subset，即他们的root是同一个，那么说明有cycle。**Example **[**4.3**](/43-graph-valid-tree.md)** , Sample code:**

   ```java
   public class Solution {
       // union find, detect cycle
       // 这里用n 表示graph所有顶点的个数，edges是int[2]的array,int[2]中2个元素，分别是edge的2个顶点
       public boolean hasCycle(int n, int[][] edges) {
           int[] roots = new int[n];
           // initialize roots array
           for (int i = 0; i < n; i++) roots[i] = i;
           for (int[] pair : edges) {
               int root1 = findRoot(roots, pair[0]);
               int root2 = findRoot(roots, pair[1]);
               // 两个vertices恰好在一个set里，cycle存在
               if(root1 == root2) return true;
               roots[root2] = roots[root1];
           }
           return false;
       }

       public int findRoot(int[] roots, int p) {
           while(roots[p] != p) {
               roots[p] = roots[roots[p]]; // optional：union-find中的path compression
               p = roots[p];
           }
           return p;
       }
   }
   ```

2. Detect **cycle** in **undirected** graph 的常规方法：[Reference](https://www.geeksforgeeks.org/detect-cycle-undirected-graph/)

3. Detect **cycle** in **directed** graph: 这里与Topological Sort常常有联系，因为Topological Sort只能用于**Directed Acyclic Graph** 中。这里也可以用一般的DFS来解决，思路就是遍历所有的vertices，对每一个vertice 做DFS，同时用一个bool\[\] visited 记录visit过的每一个点，只visit一次。同时还需要一个bool\[\] onpath记录当前这个DFS path上visit过的vertice，只有在当前path中visit到了已经到过的点才说明有cycle。在不同的path中完全有可能visit到同一个点，这是不能说明有cycle的。** 注意: 这里可以用HashMap&lt;Integer, List&lt;Integer&gt;&gt; 来表示一个tree，也可以用List&lt;List&lt;Integer&gt;&gt;来表示一个tree，index就表示vertice**。   **Example **[**4.4**](/44-course-schedule.md)**\(1\), \(2\)。**

   1 ）用Topological Sort 中的 BFS来找Cycle, **Example **[**4.4**](/44-course-schedule.md)** \(2\)** 该解法过程中需要remove edges，最后check 是不是graph 所有的edges都被remove了，如果是，则没有cycle；反之，有cycle。

   2 \)  常用的DFS 来找Cycle, **Sample Code: Complexity 就等于 DFS graph，即 O\(V+E\)**

4. ```java
   public class Solution {
       public boolean detectCycleInDirectedGraph(int n, int[][] edges) {
           boolean[] visited = new boolean[n];
           boolean[] onpath = new boolean[n];
           HashMap<Integer, List<Integer>> graph = new HashMap(); // 用hashMap来表示directed graph
           // build graph
           for(int[] edge : edges) {
               if(!graph.containsKey(edge[0])) graph.put(edge[0], new ArrayList());
               if(!graph.containsKey(edge[1])) graph.put(edge[1], new ArrayList());
               // 根据edge的方向来添加edge到hashMap中，这里表示edge从 edge[0] --> edge[1]
               graph.get(edge[0]).add(edge[1]); 
           }
           for(int i = 0; i < n; i ++) {
               if(!visited[i] && hasCycle(graph, visited, onpath, i)) return true;
           }
           return false;
       }

       public boolean hasCycle(HashMap<Integer, List<Integer>> graph, boolean[] visited, boolean[] onpath, int cur) {
           if(!graph.containsKey(cur)) return false;
           if(visited[cur]) return false;
           onpath[cur] = true;
           visited[cur] = true;
           for(int neighbor : graph.get(cur)) {
               if (onpath[neighbor] || hasCycle(graph, visited, onpath, neighbor)) return true;
           }
           // 注意结束DFS时，reset onpath中current node 为false
           // 但不用reset visited 中current node, visited记录总的visit过的vertice
           onpath[cur] = false;
           return false;
       }
   }
   ```
5. 在对graph做DFS的时候，将path中的点加入结果**List &lt;vertice&gt; rst** 中时，根据情况，有时候按顺序加入**rst.add\(current vertice\);** 有时候需要逆序reverse加入，可以用**rst.add\(0, current vertice\)。Example **[**4.6**](/46-reconstruct-itinerary.md)



