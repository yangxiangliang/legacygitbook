# 653. Two Sum IV - Input is a BST

> Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

### 思路

基本思路和一般的two sum 一样，还是用一个set记录遍历过的node的值，然后每个遍历tree node的时候，用target值减去自己的值得到需要的另外一个值k，看set里面有没有k值。有的话，则说明找到了；反之继续遍历左右node。

### 复杂度

Time: O\(n）；  Space: O\(n\)

### Code

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
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        HashSet<Integer> set = new HashSet();
        return find(root, set, k);
    }

    private boolean find(TreeNode node, HashSet<Integer> set, int k) {
        if(node == null) return false;
        if(set.contains(k - node.val)) return true;
        set.add(node.val);
        return find(node.left, set, k) || find(node.right, set, k);
    }
}
```



