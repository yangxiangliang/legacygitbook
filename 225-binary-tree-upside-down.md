# 156. Binary Tree Upside Down

> Given a binary tree where all the right nodes are either leaf nodes with a sibling \(a left node that shares the same parent node\) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.
>
> **For example**:
>
> Given a binary tree `{1,2,3,4,5},`
>
> return the root of the binary tree `[4,5,2,#,#,3,1]`

1\) 因为此处为upside down，联想到可不可以用Stack处理，感觉Stack是LIFO，有点颠倒的感觉。一开始从root开始往root.left走，把node push 入stack。然后依次pop出来，后pop出来的是之前的right child,如果pop出的node有right child，这时需要变成当前node的left child。

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
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        if (root == null) return null;
        Stack<TreeNode> stack = new Stack();
        while (root != null) {
            stack.push(root);
            root = root.left;
        }

        TreeNode newRoot = stack.pop();
        TreeNode cur = newRoot;
        while(!stack.isEmpty()) {
            TreeNode node = stack.pop();
            cur.right = node;
            if (node.right != null) {
                cur.left = node.right;
                // Trick: 这里的node.right 需要reset为Null，不然会有循环
                node.right = null;
            }
            // Trick: 同理这里的node.left 需要reset 为Null
            node.left = null;
            cur = cur.right;
        }
        return newRoot;
    }
}
```

2\) Nice recursive and iterative solutions, [explanation and details](https://discuss.leetcode.com/topic/40924/java-recursive-o-logn-space-and-iterative-solutions-o-1-space-with-explanation-and-figure)

