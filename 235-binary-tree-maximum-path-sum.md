# 124. Binary Tree Maximum Path Sum

> Given a binary tree, find the maximum path sum.
>
> For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node **and does not need to go through the root.

### 思路

要知道通过某个node的max path sum，那么需要知道以node.left为root的max path sum，以node.right为root的max path sum，再加上node.val。这里要注意的是以node.left为root的max path sum可能为negative，此时应该取其值为0，即不需要加上node.left为root的这段path。所以用recursion function来做bottom-up，在所有过程中update一个global variable max值。

### Code

```java
public class Solution {
    int max = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        getSumRootedFrom(root);
        return max;
    }
    
    public int getSumRootedFrom(TreeNode root) {
        if (root == null) return 0;
        // 注意不需要left, right值为negative，所以这里与0取较大值
        int left = Math.max(getSumRootedFrom(root.left), 0);
        int right = Math.max(getSumRootedFrom(root.right), 0);
        int rootSum = Math.max(left, right) + root.val;
        max = Math.max(max, left + right + root.val);
        return rootSum;
    }
}
```



