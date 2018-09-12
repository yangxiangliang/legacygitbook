# 236. Lowest Common Ancestor of a Binary Tree

> Given a binary tree, find the lowest common ancestor \(LCA\) of two given nodes in the tree.

此题可用recursion，从root，找p，q的LCA。LCA只有3种情况，LCA就是root，LCA在root的left subtree中，LCA在root的right subtree中。按此在root的left，right的subtree中recursion找LCA。如果遍历到 node == q或则node == p中的某一个时，便不继续往下找了，LCA只可能是该node。



### 复杂度

Time: O\(N\), N是tree node的个数。因为该方法还是会遍历所有的node。

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        // 此时说明p,q分别在root的left, right subtree中，那么LCA是root本身
        if (left != null && right != null) return root;
        return left == null ? right : left;
    }
}
```



