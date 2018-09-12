# 113. Path Sum II

> Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

这个题目思维难度不高，典型的DFS，但是有2个点容易错：

1. 每一level DFS后，需要remove点这一层加上的元素，因为code中preSum List是公用的。
2. 最后terminate的条件是\(root.left == null && root.right == null\) 的时候，而不能用\(root == null\) 判定，比如node.left != null, node.right == null的时候，用后一种方法判定，可能以为node也是leaf，但其实它不是leaf。

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
public class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> rst = new ArrayList();
        sumHelper(root, sum, rst, new ArrayList());
        return rst;
    }
    
    public void sumHelper(TreeNode root, int sum, List<List<Integer>> rst, List<Integer> preSum) {
        if (root == null) return;
        
        preSum.add(root.val);
        // 注意terminate的条件
        if (root.left == null && root.right == null) {
            if (root.val == sum) rst.add(new ArrayList<Integer>(preSum));
            // 必须remove掉此层加上的元素
            preSum.remove(preSum.size() - 1);
            return;
        }
        
        sumHelper(root.left, sum - root.val, rst, preSum);
        sumHelper(root.right, sum - root.val, rst, preSum);
        preSum.remove(preSum.size() - 1);
    } 
}
```



