# 114. Flatten Binary Tree to Linked List

> Given a binary tree, flatten it to a linked list in-place.

### 思路

容易想到recursion，如果flatten root，可以分别flatten root的left subtree和right subtree，然后将left subtree“拼接”到root.right，原来的root的right subtree“拼接”到现在root的right subtree之后。容易错的地方：left subtree和right subtree可能为null的情况。比较tricky的地方是，在flatten left subtree的时候，需要记录left subtree flatten后的last node，即leaf。因为flatten root left和right tree后，该leaf的right node需要替换为现在root的right subtree。所以在需要helper function，该function 每次recursion后需要return flatten后的最后的一个node，即leaf。

### Code

```java
public class Solution {
    public void flatten(TreeNode root) {
        flattenHelper(root);
    }

    public TreeNode flattenHelper(TreeNode root) {
    if (root == null || root.left == null && root.right == null) return root;
    TreeNode left = flattenHelper(root.left);
    TreeNode right = flattenHelper(root.right);
    // 如果left last node是null,则不用作下面的替换，root.right 还是 flatten后的root.right
    if(left != null) {
      left.right = root.right;
      root.right = root.left;
      root.left = null;
    }
    // 如果right是null，则flatten root之后的last node是root.left flatten之后的last node
    return right == null ? left: right;
  }
}
```



