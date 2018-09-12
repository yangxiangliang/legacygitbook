# 133. Clone Graph

> Clone an undirected graph. Each node in the graph contains a `label`and a list of its `neighbors`.

### 思路

就用DFS，使用一个cloneHelper function, 这个function return 某个 node clone后所对应的node。对于clone current node时，DFS clone其所有的neighbor，然后将neighbor的clone结果加入current node的clone结果的neighbors中。

### Code

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     List<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { label = x; neighbors = new ArrayList<UndirectedGraphNode>(); }
 * };
 */
public class Solution {
    HashMap<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap();

    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if( node == null) return null;
        return cloneHelper(node);
    }

    public UndirectedGraphNode cloneHelper(UndirectedGraphNode cur) {
        // 说明该node已经被clone过，直接return其 clone的结果
        if(map.containsKey(cur)) return map.get(cur);
        
        // 注意：这里就要map.put(cur, copy of cur)，不能在for循环后才把cur 和 cur的copy放入map。
        // 因为那样的话会在recursion中造成死循环。比如 1的neighbor是2，2的neighbor也有1。
        // 那么在copy 1的过程中recursion到下一层去copy 2, copy 2的时候map里没有1的copy，又会去recursion copy 1
        map.put(cur, new UndirectedGraphNode(cur.label));
        for(UndirectedGraphNode neighbor : cur.neighbors) {
            map.get(cur).neighbors.add(cloneHelper(neighbor));
        }
        return map.get(cur);
    }
}
```



