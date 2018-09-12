# 310. Minimum Height Trees

> For a undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees \(MHTs\). Given such a graph, write a function to find all the MHTs and return a list of their root labels.
>
> **Format**
>
> The graph contains `n`nodes which are labeled from `0`to `n - 1`. You will be given the number `n`and a list of undirected `edges`\(each edge is a pair of labels\).
>
> You can assume that no duplicate edges will appear in `edges`. Since all edges are undirected,`[0, 1]`is the same as     `[1, 0]`and thus will not appear together in `edges`.
>
> **Note**
>
> \(1\) According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”
>
> \(2\) The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

### 思路

**这里的思路值得学习，就是“剪枝”思路。**根据题目，我们如果将该tree的leaf看做最外层，一层一层往里走，那么**Minimum Height Trees** 的root应该是最中间的那个node最为root。由于n值的不同，最Middle的node可能有1个，或则2个。这里用List&lt;HashSet&lt;Integer&gt;&gt; graph来表示graph， index就是vertice的值，HashSet&lt;Integer&gt; 表示和这个vertice相连的其他vertice，如果该HashSet.size\(\) == 1，则其为leaf；remove 该leaf的时候，需要在与该leaf相连的vertice的HashSet中，也把该leaf remove 掉。这样一层一层remove leaf，知道最后剩下1个或则2个vertice时，便得到结果。

### Code

```java
public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<HashSet<Integer>> graph = new ArrayList();
        HashSet<Integer> nodes = new HashSet(); // 记录所有没有被remove掉的vertice
        for(int i = 0; i < n; i ++) {
            nodes.add(i);
            graph.add(new HashSet());
        }
        for(int[] pair : edges) {
            graph.get(pair[0]).add(pair[1]);
            graph.get(pair[1]).add(pair[0]);
        }
        List<Integer> leaves = new ArrayList();
        for(int i = 0; i < n ; i++) {
           if(graph.get(i).size() == 1) leaves.add(i);
        }
        
        while(nodes.size() > 2) {
            // 从nodes中remove 掉所有leaves
            nodes.removeAll(leaves);
            List<Integer> newLeaves = new ArrayList(); // 记录新的leaves
            for(int i : leaves) {
                // 因为i是leaves，所以其对应的HashSet() neighbors只有1个元素，可以用interate().next()取其值
                int neighbor = graph.get(i).iterator().next();
                // 因为remove掉i，则也需将i从i的neighbor中remove掉
                graph.get(neighbor).remove(i);
                // 记录新产生的leaves
                if(graph.get(neighbor).size() == 1) newLeaves.add(neighbor);
            }
            leaves = newLeaves;
        }
        return new ArrayList(nodes);
    }
}
```



