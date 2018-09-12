# 98. Validate Binary Search Tree

> Given a binary tree, determine if it is a valid binary search tree \(BST\).
>
> Assume a BST is defined as follows:
>
> * The left subtree of a node contains only nodes with keys **less than** the node's key.
> * The right subtree of a node contains only nodes with keys **greater than** the node's key.
> * Both the left and right subtrees must also be binary search trees.

如果是BST，inorder的时候node values应该是ascending排列的。这里就可以用inorder traversal，用一个global 的 pre node来记录visit的前一个node，pre node的value应该总是小于当前 cur node的value。

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
    TreeNode pre = null;
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        if (!isValidBST(root.left)) return false;
        if (pre != null && pre.val >= root.val) return false;
        pre = root;
        return isValidBST(root.right);
    }
}
```

### Code 2\) Iterative way

其实就是 iterative inorder traversal 过程中，判断该序列是否一直是递增序列

```java
public boolean isValidBST(TreeNode root) {
   if (root == null) return true;
   Stack<TreeNode> stack = new Stack<>();
   TreeNode pre = null;
   
   while (root != null || !stack.isEmpty()) {
      while (root != null) {
         stack.push(root);
         root = root.left;
      }
      root = stack.pop();
      if(pre != null && root.val <= pre.val) return false;
      pre = root;
      root = root.right;
   }
   return true;
}
```



