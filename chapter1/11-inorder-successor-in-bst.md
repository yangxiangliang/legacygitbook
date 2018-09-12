# 285. Inorder Successor In BST

> Given a binary search tree and a node in it, find the in-order successor of that node in the BST.
>
> Note: If the given node has no in-order successor in the tree, return null.

对于一个 BST 进行Inorder Traversal 得到的结果是将 node values 进行 ascending order 排列

### 1\) Recursive

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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if (root == null) return null;
        // 如果root.val <= p.val, inorder successor 一定在root.right subtree上，也可能是null
        // 反之，去root.left subtree 找p的Inorder successor, 此时root是p的possible inorder successor
        if (root.val <= p.val) {
            return inorderSuccessor(root.right, p);
        } else {
            TreeNode tmp = inorderSuccessor(root.left, p);
            // Trick: Maybe there is no successor in root's left tree for p
            // At this time, root should be the successor
            return (tmp != null) ? tmp : root;
        }
    }
}
```

### 2\) Iterative

比较root, p 的value大小，如果root更大，则root是possible inorder successor, 然后左移root继续找。如果root更小，右移去找更大的node，此时不update possible inorder successor candidate

```java
public class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if (root == null) return null;
        TreeNode successor = null;
        while(root != null) {
            if (p.val < root.val) {
                successor = root;
                root = root.left;
            } else root = root.right;
        }
        return successor;
    }
}
```



