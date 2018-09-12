# 312. Burst Balloons

> Given`n`balloons, indexed from`0`to`n-1`. Each balloon is painted with a number on it represented by array`nums`. You are asked to burst all the balloons. If the you burst balloon`i`you will get`nums[left] * nums[i] * nums[right]`coins. Here`left`and`right`are adjacent indices of`i`. After the burst, the`left`and`right`then becomes adjacent.
>
> Find the maximum coins you can collect by bursting the balloons wisely.
>
> **Note:**  
> \(1\) You may imagine`nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them.  
> \(2\) 0 ≤`n`≤ 500, 0 ≤`nums[i]`≤ 100
>
> **Example:**
>
> Given`[3, 1, 5, 8]`
>
> Return`167`
>
> ```
> nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] -->[]
> coins =  3*1*5    +  3*5*8    +  1*3*8  + 1*8*1  = 167
> ```

## 思路

1. 一拿到这个题目，容易想到从一开始\[3, 1, 5, 8\]中可以4个选择burst一个，然后从从剩下的3个中选择一个burst，以此类推来求得最大值。这样的复杂度是\(N!\)，因为最开始有N种选择，然后有\(N-1\)种选择，\(N-2\)种选择。。。最后一共有\(N!\)种情况需要计算到，时间复杂度是\(N!\)。\(N!\)的时间复杂度太大了，想进一步优化。
2. 最容易想到的就是\(N!\)中会不会有很多重复计算，那么可以用memorize一下，来降低复杂度。如果将每个位置用\(0, 1\)来表示，0表示burst了，1表示没有burst，那么一共也有\(2^N\)种情况计算，时间复杂度也至少是\(2^N\)。还是复杂度太大。想进一步优化。
3. **DP的时候，有一个模板\(常用思路\)就是 “倒推”，即从“最终状态”往前推理。**这里可以想，如果最后放的是5，那么最后burst的结果是 5 \* 1 \* 1，然后5相当于把左边和右边“隔离”开了，可以把这个看成左边\[3, 1\] burst ballons 是原问题的子问题，右边\[8\] 也是该问题的子问题，分别求出这2个子问题的结果，然后加上burst 5的结果即可。**注意：最后burst 5的时候，左边和右边都已经超出边界，边界相当于是1。burst \[3, 1\]的时候，右边其实是5，边界不是1。相当于....\[3, 1\].... 只是原来问题当中的一段，其实  \[3, 1\] 左边和右边都有其他的气球。** 用一个二维数组int\[start\]\[end\]， start, end分别表示子问题的左右边界，来记录各个子问题的最大值即可。要计算完全int\[start\]\[end\]当中所有情况，即时间复杂度是O\(N^2\)。
4. **这里用到了“DP”解法中常用的“倒推”模板，即从“终局”状态往前面开始倒推，该方法容易将时间复杂度从“指数级”降低为\(N^2\)**

### Code

```java
class Solution {
    public int maxCoins(int[] nums) {
        int[][] map = new int[nums.length][nums.length];
        return maxCoins(nums, 0, nums.length-1, map);
    }

    private int maxCoins(int[] nums, int start, int end, int[][] map) {
        if(start > end)  return 0;
        if(map[start][end] > 0) return map[start][end]; // 表示已经在memorize的map中了
        int rst = 0;
        for(int i = start; i <= end; i ++) {
            // start == 0，end == nums.length-1的时候，相当于左右2个边界气球值是1
            int leftBound = 1, rightBound = 1;
            if(start > 0) leftBound = nums[start-1];
            if(end < nums.length-1) rightBound = nums[end+1];
            // 注意：start, end只是原问题中一段，最后放气球i的时候，增加的值不是nums[i]*1*1, 是 nums[i]*nums[start-1]*nums[end+1]
            rst = Math.max(rst, nums[i]*leftBound*rightBound + maxCoins(nums, i+1, end, map) + maxCoins(nums, start, i-1, map));
        }
        map[start][end] = rst;
        return map[start][end];
    }
}
```



