# 222. Count Complete Tree Nodes

> Given a **complete **binary tree, count the number of nodes.
>
> **Definition of a complete binary tree from Wikipedia:**
>
> In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2^h nodes inclusive at the last level h.



注意这里是complete tree，利用complete tree的特点。先找出root的height，因为complete tree最后一层都是as left as possible，所以找height的时候，一直往left node 遍历，直到null。然后计算root.right node的height，如果height\(root.right\) == height\(root\) -1,说明right subtree有leaf在last level，所以root的left subtree的最后一层是满的。由此可得到left subtree中节点个数是 2^\(height\(root.left\)\) - 1, 用recursion，该值 + 1\(root node\) + right tree的节点个数 即可。反之，right subtree没有leaf在last level，说明right subtree在倒数第二level也是满的，同理recursion，可以得到总的节点个数。



```java
public class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        int h = getHeight(root);
        int rightHeight = getHeight(root.right);
        // in this case, means right subtree has leaf on last level
        // total nodes of left subtree is 2^(h-1) - 1, add 1 more root node, which is 2^(h-1)
        if (h - 1 == rightHeight) return (1<<rightHeight) + countNodes(root.right);
        // right tree does not has leaf on last level, total right tree nodes is 2^(h-2) - 1
        // add one more root node, which is 2^(h-2)
        else return (1<<rightHeight) + countNodes(root.left);
    }

    public int getHeight(TreeNode root) {
        int height = 0;
        while(root != null) {
            root = root.left;
            height ++;
        }
        return height;
    }
}
```



