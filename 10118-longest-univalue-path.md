# 687. Longest Univalue Path

> Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.
>
> **Note:** The length of path between two nodes is represented by the number of edges between them.
>
> **Example 1:**
>
> Input:
>
> ```
>               5
>              / \
>             4   5
>            / \   \
>           1   1   5
> ```
>
> Output: 2
>
> **Example 2:**
>
> Input:
>
> ```
>               1
>              / \
>             4   5
>            / \   \
>           4   4   5
> ```
>
> Output: 2

### 思路

1. 首先思考哪些情况可以组成Longest univalue path。一种情况就是从某个parent往下一直到某个children；还有一种情况是某个parent其左边和右边的值和parent的值都相同，形成的path中parent不是端点。
2. 那么需要一个函数，来计算从某node开始，值为val的连续path最大长度。在计算的过程中有1个variable来记录最后可能组成的最大结果，此时往往采用 global variable 来update即可。

### 复杂度

Time: O\(N\)，N是binary tree的nodes的个数，因为该方法还是会touch每一个node。

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

    int rst = 0;
    public int longestUnivaluePath(TreeNode root) {
        if(root == null) return 0;
        getLength(root, root.val);
        return rst;
    }

    // getLength 表示从root 开始值为val的node的连续path最大长度，root是该path的端点，path不可以“经过”root而把其当做其中一点
    private int getLength(TreeNode root, int val) {
        if(root == null) return 0;
        int left = getLength(root.left, root.val);
        int right = getLength(root.right, root.val);
        rst = Math.max(rst, left + right);
        if(val == root.val) return Math.max(left, right) + 1;
        return 0;
    }
}
```



