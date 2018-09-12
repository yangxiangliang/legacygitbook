# 198. House Robber

> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
>
> Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

### 思路

1. 想到偷的过程，比如第1个房间可以有2种情况，可以偷，可以不偷。如果偷1，那么最后结果就是\(1中的金子+偷\[3, 4, 5.....\]\)的结果，因为房间2不能偷。如果不偷1，那么结果就是偷\[2, 3, 4.....\] 的结果。感觉偷原来的房间int\[\] nums的结果取决于偷其sub array\(sub 房间\)的结果。由此想到构造解空间树，只需要求得偷\[x, y, z....., nums\[nums.length-1\]\]中的最大值即可。因此用一个memo来记录解空间树的结果，memo\[i\]表示从index 为i的房间到最后房间可以偷得的金子的最大值。这里解空间树中个数为O\(N\)，所以时间复杂度也是O\(N\)，空间复杂度也是O\(N\)。

### Code

#### 1\)

```java
class Solution {
    public int rob(int[] nums) {
        int[] memo = new int[nums.length];
        return rob(nums, 0, memo);
    }

    private int rob(int[] nums, int index, int[] memo) {
        if(index > nums.length - 1) return 0;
        if(memo[index] > 0) return memo[index];
        // 如果current index的房间金子 == 0, 那么不用偷该房间，直接从下一个房间开始偷
        if(nums[index] == 0) memo[index] = rob(nums, index+1, memo);
        // 分2种情况：(1) 偷该房间, 从该房间后面的2个房间继续开始偷；(2)不偷该房间，从下一个房间开始偷
        else memo[index] = Math.max(nums[index] + rob(nums, index+2, memo), rob(nums, index+1, memo));
        return memo[index];
    }
}
```

#### 2\) 优化

1. 由于每个房间的数值肯定都需要过一遍，所以时间复杂度是O\(N\)不可能优化。现在想一想有没有可能优化Space，即不用一个长度为N的数组memo来记录所有的情况。
2. 对于每一个房间只有“偷”和“不偷”的情况，决定该房间能不能偷的是前面一个房间是否被偷的情况，那么可不可以用2个variable来分别记录前面房间被偷与否的情况下金子值。然后在遍历所有房间的时候，每次再update这2个variable，最后取其中的较大值即可。

**Time O\(N\), Space O\(1\)**

```java
class Solution {
    public int rob(int[] nums) {
        int preNo = 0; // 表示没有偷之前房间的情况下得到的最大金子值
        int preYes = 0; // 表示偷之前房间的情况下得到的最大金子值
        for(int num : nums) {
            int temp = preNo;
            // update preNo，表示不偷当前房间，所以取之前的preNo, preYes的最大值
            preNo = Math.max(preNo, preYes);
            // update preYes, 表示偷当前房间，所以之前的房间不能偷，取update preNo之前的值 + 当前房间金子值
            preYes = num + temp;
        }
        return Math.max(preNo, preYes);
    }
}
```



