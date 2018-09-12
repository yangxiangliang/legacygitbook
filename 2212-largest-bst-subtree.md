# 333. Largest BST Subtree

> Given a binary tree, find the largest subtree which is a Binary Search Tree \(BST\), where largest means subtree with largest number of nodes in it.
>
> **Note:   **A subtree must include all of its descendants.
>
> **Follow up:  **Can you figure out ways to solve it with O\(n\) time complexity ?

此题也可用**Bottom-up**的思路，因为_a subtree must include all of its descendants_，所以从最下面leaf开始往上找最大的BST。而且BST的性质，如果root为BST，那么root的left和right subtree都必须是BST。所以**bottom-up**的时候，如果一个tree的left 或则right subtree中有不是BST，那么该tree也不可能是BST。所以用一个helper function 来count 以root 开始的BST 的node的个数，如果以root开始的tree不是BST，我们**可以return  -1**，这样所有将该tree 作为subtree的tree都不可能是BST。



1）判断root的tree是BST，除了其left和right subtree都必须是BST以外，还要检查root.val必须小于right subtree的最小值，大于left subtree的最大值。

```java
public class Solution {
    int max = 0;

    public int largestBSTSubtree(TreeNode root) {
        countBST(root);
        return max;
    }

    public int countBST(TreeNode root) {
        if (root == null) return 0;
        int left = countBST(root.left);
        int right = countBST(root.right);
        if (left == -1 || right == -1) return -1;
        if(root.val > getMax(root.left) && root.val < getMin(root.right)) {
            max = Math.max(max, 1 + left + right);
            return 1 + left + right;
        }
        return -1;
    }

    public int getMax(TreeNode node) {
        // return Integer.MIN_VALUE, 这样每个root.val都可以大于该值
        if (node == null) return Integer.MIN_VALUE;
        while(node.right != null) node = node.right;
        return node.val;
    }

    public int getMin(TreeNode node) {
        if (node == null) return Integer.MAX_VALUE;
        while(node.left != null) node = node.left;
        return node.val;
    }
}
```



2）每一次检查一个root 的tree是不是BST，都要去找其left，right subtree的最大值，以及最小值，这样很**inefficient**。所以构建一个class，里面有root的tree的BST的size，以及该tree的最大值和最小值。当然如果该tree不是BST，那么其size为-1.

```java
public class Solution {
    class Result {
        int size;
        int min;
        int max;
        Result(int size, int min, int max) {
            this.size = size;
            this.min = min;
            this.max = max;
        }
    }

    int max = 0;

    public int largestBSTSubtree(TreeNode root) {
        countBST(root);
        return max;
    }

    public Result countBST(TreeNode root) {
        if (root == null) {
            return new Result(0, Integer.MAX_VALUE, Integer.MIN_VALUE);
        }

        Result left = countBST(root.left);
        Result right = countBST(root.right);
        if (left.size == -1 || right.size == -1 || root.val <= left.max || root.val >= right.min) {
            return new Result(-1, Integer.MAX_VALUE, Integer.MIN_VALUE);
        }

        int size = 1 + left.size + right.size;
        max = Math.max(max, size);
        return new Result(size, Math.min(left.min, root.val), Math.max(right.max, root.val));
    }
}
```

因为这里还是会access every node，所以 Time Complexity 为O\(N\)

