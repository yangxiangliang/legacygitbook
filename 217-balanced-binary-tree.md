# 110. Balanced Binary Tree

> Given a binary tree, determine if it is height-balanced.
>
> For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

这个问题基本有两个解决方法，1是top down way；2是 bottom up way

1\) top down way, 用helper function，先计算left subtree和right subtree的height，他们的height相差不能超过1。同时left, right subtree都必须是 balanced。

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
    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        int left = height(root.left);
        int right = height(root.right);
        return Math.abs(left - right) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }
    
    public int height(TreeNode root) {
        if (root == null) return 0;
        return Math.max(height(root.left), height(root.right)) + 1;
    }
}
```

Time: call height\(root\)函数的时候需要access all children， 所以是O\(N\)。但是因为要检查root.left, root.right是不是isBalanced，他们也需要call height\(root\)，即所有node都需要检查是不是balanced。最后的complexity 是O\(N^2\)。

2\) bottom up way。对于每一个node，不用计算他们的height，可以return 一个integer，如果这个node为root的subtree是balanced，就return 其height，如果其不是balanced，return -1。这样recursion时，这个node的parent nodes可以根据return的值来判定其是不是balanced。

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
    public boolean isBalanced(TreeNode root) {
        return dfsHeight(root) != -1;
    }
    
    public int dfsHeight(TreeNode root) {
        if (root == null) return 0;
        int left = dfsHeight(root.left);
        if (left == -1) return -1;
        int right = dfsHeight(root.right);
        if (right == -1) return -1;
        return (Math.abs(left - right) > 1) ? -1 : Math.max(left, right) + 1;
    }
}
```

Time:  这个recursion会access all nodes, 所以TC是O\(N\)

