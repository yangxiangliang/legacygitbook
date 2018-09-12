# 337. House Robber III

> The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.
>
> Determine the maximum amount of money the thief can rob tonight without alerting the police.

### 思路

这道题目的关键就是分2种情况，\(1\) 如果root被rob了，那么root.left, root.right 不能被rob，只能找root.left，root.right的subtree的被rob的最大值；\(2\) 如果root没有被rob，那么rob root的最大值就是rob\(root.left\)，rob\(root.right\)的最大值之和。考虑到重复计算，可以用一个HashMap来记录各个node和rob 该node取得的最大值。

### 复杂度

Time: O\(N\)。Space:O\(N\)。N是binary tree中node的个数。

这里recursion，会touch到每一个node，直到最bottom node，然后一层一层往上，每一层recursion计算时间根据code很容易得知是constant time，又因为map记录，每一个node只被计算一次，所以总的complexity就是 O\(N\)

### Code 1\)

```java
public class Solution {
    HashMap<TreeNode, Integer> map = new HashMap();

    public int rob(TreeNode root) {
        if (root == null) return 0;
        if (map.containsKey(root)) return map.get(root);
        // Do not count root.val
        int noRoot = rob(root.left) + rob(root.right);
        // Count root.val
        int hasRoot = root.val;
        if (root.left != null) hasRoot += rob(root.left.left) + rob(root.left.right);
        if (root.right != null) hasRoot += rob(root.right.left) + rob(root.right.right);
        map.put(root, Math.max(noRoot, hasRoot));
        return map.get(root);
    }
}
```

进一步想，这里的关键就是分以上2种情况，那么可不可以用一个helper function 同时return以上2种情况的数值，那么recursion的时候，上一层recursion的计算可以同时利用下一层 return的这2个数值。所以我们需要采用int\[2\]数组来记录，第一个是记录root 没有被rob的情况下的最大值，第二个是记录root被rob的情况下的最大值。

### Code 2\)

```java
public class Solution {
    public int rob(TreeNode root) {
        int[] rst = robHelper(root);
        return Math.max(rst[0], rst[1]);
    }
    public int[] robHelper(TreeNode root) {
        if (root == null) return new int[2];
        int[] left = robHelper(root.left);
        int[] right = robHelper(root.right);
        int[] rst = new int[2];
        // root 没有被rob的情况，则root.left, root.right可能被rob，也可能不被rob
        rst[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        // root 被rob的情况，则root.left, root.right不能被rob
        rst[1] = root.val + left[0] + right[0];
        return rst;
    }
}
```



