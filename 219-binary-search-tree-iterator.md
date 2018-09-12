# 173. Binary Search Tree Iterator

> Implement an iterator over a binary search tree \(BST\). Your iterator will be initialized with the root node of a BST.
>
> Calling `next()`will return the next smallest number in the BST.
>
> **Note: **`next() and hasNext()`should run in average O\(1\) time and uses O\(h\) memory, where h is the height of the tree.

### 思路

利用一个Stack，想到inorder traversal的结果就是 node values按ascending排列，刚好下一个就是next smallest value。inorder traversal的iterative方法里，可以用stack来处理。这里可以借鉴这种方法，一开始从root 往left node走，然后一个一个push to stack，直到left leaf。Pop node出来的时候，用同样的方法将node的right node的所有left node也push 进去。

### 复杂度

这里hasNext\(\)的complexity为O\(1\)好理解；乍一看next\(\)的complexity貌似不是O\(1\)，但是仔细想，因为不是每个node的node.right都有left node可以push，next\(\)操作的amortized complexity是O\(1\)。也可以理解为每个node只进入或则出stack一次，所以amortized complexity是O\(1\)

### Code

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class BSTIterator {
    Stack<TreeNode> stack = new Stack();

    public BSTIterator(TreeNode root) {
        pushNodes(root);
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty();
    }

    /** @return the next smallest number */
    public int next() {
        TreeNode node = stack.pop();
        pushNodes(node.right);
        return node.val;
    }

    public void pushNodes(TreeNode node) {
        if (node == null) return;
        while(node != null) {
            stack.push(node);
            node = node.left;
        }
    }
}

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = new BSTIterator(root);
 * while (i.hasNext()) v[f()] = i.next();
 */
```



