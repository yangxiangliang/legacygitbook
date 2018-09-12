# 235. Lowest Common Ancestor of a Binary Search Tree

> Given a binary search tree \(BST\), find the lowest common ancestor \(LCA\) of two given nodes in the BST.
>
> According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor) : “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants \(where we allow **a node to be a descendant of itself**\).”

从root开始向下查找，如果2个node都比root.val小，继续往左查找；如果2个node都比root.val大，继续往右查找；如果两个node分别在root两边，则此时root为他们的LCA。

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
        while (root != null) {
            if (root.val < p.val && root.val < q.val) root = root.right;
            else if (root.val > p.val && root.val > q.val) root = root.left;
            else return root;
        }
        return null;
    }
}
```

Time: O\(h\), h为树的高度。 Space: O\(1\)



