# 501. Find Mode in Binary Search Tree

> Given a binary search tree \(BST\) with duplicates, find all the mode\(s\) \(the most frequently occurred element\) in the given BST.
>
> **Note: **If a tree has more than one mode, you can return them in any order.
>
> **Follow up: **Could you do that without using any extra space? \(Assume that the implicit stack space incurred due to recursion does not count\).



注意这里是BST，**BST的特点联想到inorder traversal的时候node value是non-descending排列的。如果是modes，那么在inorder traversal的时候相等的node是连在一起的**。所以用 inorder traversal，然后用一个pre node记录traversal的上一个node，然后用count记录与current node相等的node的个数，另一个global variable max 记录目前modes出现的次数。

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
    int count = 1;
    int max = 0;
    TreeNode pre = null;
    
    public int[] findMode(TreeNode root) {
        List<Integer> modes = new ArrayList();
        inorderTraversal(root, modes);
        int[] rst = new int[modes.size()];
        for (int i = 0; i < modes.size(); i ++) {
            rst[i] = modes.get(i);
        }
        return rst;
    }
    
    // parse List<Integer> modes 方便inorderTraversal时候加/减/清空 modes
    public void inorderTraversal(TreeNode root, List<Integer> modes) {
        if (root == null) return;
        inorderTraversal(root.left, modes);
        if (pre != null) {
            if (pre.val == root.val) {
                count ++;
            // 说明第一次遇见current node的value,所以count设置为1
            } else count = 1;
        }
        if (count > max) {
            modes.clear();
            modes.add(root.val);
            max = count;
        } else if (count == max) modes.add(root.val);
        pre = root;
        inorderTraversal(root.right, modes);
    }
}
```



