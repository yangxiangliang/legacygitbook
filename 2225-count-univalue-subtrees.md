# 250. Count Univalue Subtrees

> Given a binary tree, count the number of uni-value subtrees.
>
> A Uni-value subtree means all nodes of the subtree have the same value.

### 思路

如果一个tree是Univalue，则说明tree的left和right subtree都是Univalue，而且值和该tree的root的值相等。容易想到这个题目可以Bottom-up，因为在bottom的subtree是Univalue的，那么往上走的时候更大的tree才可能是Univalue的。而且用一个helper function 来return其subtree是不是Univalue的，同时用一个global variable 来count所用找到Univalue的subtree的个数。

### Code

```java
public class Solution {
    int count = 0;

    public int countUnivalSubtrees(TreeNode root) {
        helper(root);
        return count;
    }
    public boolean helper(TreeNode node) {
        if (node == null) return true;
        boolean left = helper(node.left);
        // 这里必须让helper() function遍历所有的node.left, node.right subtrees
        // 不能写成if(helper(node.left) && helper(node.right)) {}，因为helper(node.left) is false
        // 那么不会进入function helper(node.right)，最后的count会变少
        boolean right = helper(node.right);
        if(left && right) {
            if (node.left != null && node.left.val != node.val) return false;
            if (node.right != null && node.right.val != node.val) return false;
            count ++;
            return true;
        }
        return false;
    }
}
```



