# 99. Recover Binary Search Tree

> Two elements of a binary search tree \(BST\) are swapped by mistake.
>
> Recover the tree without changing its structure.
>
> **Note:**
>
> A solution using O\(n\) space is pretty straight forward. Could you devise a constant space solution?

### 思路

由于BST的性质是inorder traversal是sorted的，所以这里还是用inorder traversal来解决，用一个global pointer pre来记录遍历的cur node的上一个node，n1, n2分别表示swap的那两个node，假设n1在inorder的靠前部分，n2在inorder的靠后部分。如果pre.val &gt; cur.val，这时如果n1是Null，那么pre 就是n1；cur 可能是n2，也可能不是，因为除非n1，n2在inorder traversal中相邻一起，其他情况中，n2还在inorder traversal的靠后面部分。总之凡是遇见pre.val &gt; cur.val的情况，都赋值cur给n2直到最后。

### Code

```java
public class Solution {
    TreeNode pre = null;
    TreeNode n1 = null;
    TreeNode n2 = null;
    
    public void recoverTree(TreeNode root) {
        inorder(root);
        int tmp = n2.val;
        n2.val = n1.val;
        n1.val = tmp;
    }
    
    public void inorder(TreeNode root) {
        if (root == null) return;
        inorder(root.left);
        if (pre != null && pre.val > root.val) {
            if (n1 == null) {
                n1 = pre;
            }
            n2 = root;
        }
        pre = root;
        inorder(root.right);
    }
}
```



