# 538. Convert BST to Greater Tree

> Given a Binary Search Tree \(BST\), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

### 思路

此题不用convert后的tree也是BST，当然也不会是BST。容易想到BST的inorder 是递增的，从这个数列的后面往前遍历，便容易用一个variable 记录比某值大的所有数的sum，往前遍历一个时，在这个sum基础上加上current元素的值即可。联想到，所以这里需要用和inorder顺序相反的遍历，即先right node，再root, 最后left node，然后用一个sum记录目前为止，大于该node的所有数的和。

### Code

```java
public class Solution {
    int sum = 0;

    public TreeNode convertBST(TreeNode root) {
        getSum(root);
        return root;
    }
    // inorder is visiting tree from small to big
    // we should use reverse of inorder
    public void getSum(TreeNode root) {
        if (root == null) return;
        getSum(root.right);
        int original = root.val; // copy original value of current node
        root.val = sum + root.val;
        sum += original; // change sum after current node value changed
        getSum(root.left);
    }
}
```



