# 314. Binary Tree Vertical Order Traversal

> Given a binary tree, return the vertical order traversal of its nodes' values. \(ie, from top to bottom, column by column\).
>
> If two nodes are in the same row and column, the order should be from **left to right**.

### 思路

因为最后输出的顺序是 from top to bottom, from left to right。则想到从上到下用level order traversal，即BFS遍历所有node。而且遍历的时候需要记录当前node是属于哪一个column的，然后用 hashMap 存每个column对应的list&lt;Integer&gt;的值。

### Code

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    class Guide {
        TreeNode node;
        int col;

        public Guide(TreeNode _node, int _col) {
            node = _node;
            col = _col;
        }
    }

    public List<List<Integer>> verticalOrder(TreeNode root) {
        if(root == null) return new ArrayList();

        HashMap<Integer, List<Integer>> map = new HashMap();
        Queue<Guide> queue = new LinkedList();
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        queue.offer(new Guide(root, 0));

        while(!queue.isEmpty()) {
            Guide cur = queue.poll();
            min = Math.min(min, cur.col);
            max = Math.max(max, cur.col);

            if(!map.containsKey(cur.col)) map.put(cur.col, new ArrayList());
            map.get(cur.col).add(cur.node.val);

            if(cur.node.left != null) queue.offer(new Guide(cur.node.left, cur.col - 1));
            if(cur.node.right != null) queue.offer(new Guide(cur.node.right, cur.col + 1));
        }

        List<List<Integer>> rst = new ArrayList();
        for(int i = min; i <= max; i ++) {
            rst.add(map.get(i));
        }
        return rst;
    }
}
```

### 思路 2）

如果不要求每一column中integer的顺序的话，就不用level by level 从上往下BFS了，可以直接DFS来做。然后用TreeMap 记录每个column 值以及其对应column的所有integer 值，用treeMap应为其是按照key 排好序的，所以最后结果遍历treeMap的时候，直接得到从最左边column 到 最右边column的所有integer的lists

### Code 2\)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        TreeMap<Integer, List<Integer>> map = new TreeMap();
        dfs(root, map, 0);
        
        List<List<Integer>> rst = new ArrayList();
        for(int col : map.keySet()) rst.add(map.get(col));
        return rst;
    }
    
    private void dfs(TreeNode root, TreeMap<Integer, List<Integer>> map, int col) {
        if(root == null) return;
        if(!map.containsKey(col)) map.put(col, new ArrayList<>());
        
        map.get(col).add(root.val);
        dfs(root.left, map, col - 1);
        dfs(root.right, map, col + 1);
    }
}
```



