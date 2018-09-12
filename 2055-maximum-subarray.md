# 53. Maximum Subarray

> Given an integer array `nums`, find the contiguous subarray \(containing at least one number\) which has the largest sum and return its sum.
>
> **Example:**
>
> ```
> Input: [-2,1,-3,4,-1,2,1,-5,4],
> Output: 6
> Explanation: [4,-1,2,1] has the largest sum = 6.
> ```
>
> **Follow up:**
>
> If you have figured out the O\(_n_\) solution, try coding another solution using the divide and conquer approach, which is more subtle.

### 思路

1. 首先观察怎么才能获得max subarray。对于input int\[\] nums 而言，如果nums\[i\] 在max subarray中，那么nums\[i\] 之前那部分的continuous subarray 肯定是需要大于0的，不然其加上 nums\[i\] 反而让 nums\[i\] 的值更小了。

### Code

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = nums[0];
        int rst = nums[0];
        
        for(int i = 1; i < nums.length; i ++) {
            pre = Math.max(pre, 0);
            pre += nums[i];
            rst = Math.max(rst, pre);
        }
        return rst;
    }
}
```



