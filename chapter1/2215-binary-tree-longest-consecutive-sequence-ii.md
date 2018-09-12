# 549. Binary Tree Longest Consecutive Sequence II

> Given a binary tree, you need to find the length of Longest Consecutive Path in Binary Tree.
>
> Especially, this path can be either increasing or decreasing. For example, \[1,2,3,4\] and \[4,3,2,1\] are both considered valid, but the path \[1,2,4,3\] is not valid. On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

此题需要找到从任何一个node为起点的consecutive sequence的个数，而且sequence可能是increasing，decreasing 两种情况。容易想到，从**bottom\(leaf\)**开始计算比较容易，然后可以用**bottom-up**的思路。**由于要return从该node起increasing，decreasing两种情况sequence 的个数，所以helper function 返回的值用数组 int\[\] 代替。**

Time: O\(N\)

```java
public class Solution {
    int max = 0;

    public int longestConsecutive(TreeNode root) {
        longestPath(root);
        return max;
    }
    // count longest consecutive sequence rooted at root node
    public int[] longestPath (TreeNode root) {
        if (root == null) return new int[]{0, 0};
        int dcCount = 1, inCount = 1;

        if (root.left != null) {
            int[] l = longestPath(root.left);
            if (root.val == root.left.val + 1) dcCount = 1 + l[0];
            else if (root.val == root.left.val - 1) inCount = 1 + l[1];
        }

        if (root.right != null) {
            int[] r = longestPath(root.right);
            if (root.val == root.right.val + 1) dcCount = Math.max(dcCount, 1 + r[0]);
            else if (root.val == root.right.val - 1) inCount = Math.max(inCount, 1 + r[1]);
        }
        max = Math.max(max, dcCount + inCount - 1); // count root.val twice, so need to "-1" to remove duplicate counts
        return new int[]{dcCount, inCount};
    }
}
```



