# 450. Delete Node in a BST

> Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference \(possibly updated\) of the BST.
>
> Basically, the deletion can be divided into two stages:
>
> 1. Search for a node to remove.
> 2. If the node is found, delete the node.
>
> **Note:**Time complexity should be O\(height of tree\).

1\) Iterative。首先如果target的node存在的话，找到这个value等于key的node。然后关键是implement一个function 去delete这个node，此时该node是其subtree的root，即delete该subtree的root。delete的时候，这里的方法是需要找的该subtree中比node的值大的最小值的nextNode，然后将nextNode 与node互换。

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
    public TreeNode deleteNode(TreeNode root, int key) {
        // 需要一个pre node来记录cur node的parent node，之后delete cur node的时候需要用
        // 用法：pre node不用create dummy node, 用Null 初始化即可
        TreeNode pre = null;
        TreeNode cur = root;

        // find node with key
        while(cur != null && cur.val != key) {
            pre = cur;
            if (cur.val < key) cur = cur.right;
            // 这里需要用else if,不能初心用if，因为上面cur已经变化，如果用if,可能这两个if语句都是true
            else if (cur.val > key) cur = cur.left;
        }
        if (pre == null) return deleteRootNode(cur);

        if (pre.left == cur) pre.left = deleteRootNode(cur);
        else pre.right = deleteRootNode(cur);
        return root;
    }

    // 关键是这个function
    public TreeNode deleteRootNode(TreeNode root) {
        if (root == null) return null;
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;

        // 记录next node的之前一个node
        TreeNode preNext = null;
        TreeNode next = root.right;
        while(next.left != null) {
            preNext = next;
            next = next.left;
        }
        next.left = root.left;
        // if root.right == next, 直接return next即可
        if (root.right != next) {
            preNext.left = next.right;
            next.right = root.right;
        }
        return next;
    }
}
```

Time : O\(height of tree\), Space: O\(1\)

2\) Recursive。更好理解，但是Space是O\(height of tree\), Time仍然是O\(height of tree\)。基本思路是：比如Key &gt; cur.val，则等于key的node在cur的right subtree，recursive 从cur的right subtree中 delete node，return delete后的subtree的new root，然后将cur node的right child 赋值成 new root即可。反之，对cur 的left subtree做此操作。

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
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        if (key < root.val) root.left = deleteNode(root.left, key);
        else if (key > root.val) root.right = deleteNode(root.right, key);
        else {
            if (root.right == null) return root.left;
            int min = getMin(root.right);
            root.val = min;
            // (难点) 利用deleteNode函数来remove掉 root的right subtree中最小的node
            root.right = deleteNode(root.right, min);
        }
        return root;
    }

    public int getMin(TreeNode node) {
        while(node.left != null) {
            node = node.left;
        }
        return node.val;
    }
}
```



