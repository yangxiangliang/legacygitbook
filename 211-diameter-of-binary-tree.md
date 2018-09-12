# 543. Diameter of Binary Tree

> Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest **path between any two nodes in a tree. This path may or may not pass through the root.
>
> **Note**: The length of path between two nodes is represented by the number of edges between them.



需要一个另外的function来计算每个node的maxDepth, 这是一个recursion。在计算每个node的maxDepth的时候，维护一个global variable 来记录有可能的maxDiameter。因为maxDiameter肯定是某个node的 maxDepth\(node.left\)+1+maxDepth\(node.right\)+1。

注意这里如果Node没有left, right node，则其depth是0。如果node == null, 其depth应该是 -1。

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
    int max = 0;
    
    public int diameterOfBinaryTree(TreeNode root) {
        maxDepth(root);
        return max;
    }
    public int maxDepth(TreeNode root) {
        if (root == null) return -1;
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        max = Math.max(max, left + right + 2);
        return Math.max(left, right) + 1;
    }
}
```





