# 270. Closest Binary Search Tree Value

> Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.
>
> **Note:**
>
> * Given target value is a floating point.
> * You are guaranteed to have only one unique value in the BST that is closest to the target.

### 思路

思路容易，就从root开始找与target最接近的value。如果node.val &lt; target，那么node移动到node.right，反之移动到node.left。

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
public class Solution {

    // Iterative
    public int closestValue(TreeNode root, double target) {
        double gap = Double.MAX_VALUE;
        int rst = 0;
        while(root != null) {
            if (Math.abs(root.val - target) < gap) {
                gap = Math.abs(root.val - target);
                rst = root.val;
            }
            if (target >= (double) root.val) root = root.right;
            else root = root.left;
        }
        return rst;
    }
}
```



