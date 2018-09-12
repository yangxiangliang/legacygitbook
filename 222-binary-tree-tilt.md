# 563. Binary Tree Tilt

> Given a binary tree, return the tilt of the **whole tree**. The tilt of a **tree node **is defined as the **absolute difference **between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.
>
> The tilt of the **whole tree **is defined as the sum of all nodes' tilt.



1\) 和题目2.1.1类似，用一个function得来每个node以及其所有子node的value之和。在每次recursion过程中，update global variable 来记录总的tilt，然后用一个hashMap来记录已经计算过的node及其子node的value之和，避免重复计算。

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
    int tilt = 0;
    HashMap<TreeNode, Integer> map = new HashMap();
    
    public int findTilt(TreeNode root) {
        getSum(root);
        return tilt;
    }
    
    public int getSum(TreeNode root) {
        if (root == null) return 0;
        if (map.containsKey(root)) return map.get(root);
        int left = getSum(root.left);
        int right = getSum(root.right);
        tilt += Math.abs(left-right);
        map.put(root, left + right + root.val);
        return map.get(root);
    }
}
```

2\) 仔细分析过程后，上面解法中的hashMap没有用处，完全可以去掉。

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
    int tilt = 0;
    
    public int findTilt(TreeNode root) {
        getSum(root);
        return tilt;
    }
    
    public int getSum(TreeNode root) {
        if (root == null) return 0;
        int left = getSum(root.left);
        int right = getSum(root.right);
        tilt += Math.abs(left-right);
        return left + right + root.val;
    }
}
```



