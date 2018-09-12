# 487. Max Consecutive Ones II

> Given a binary array, find the maximum number of consecutive 1s in this array if you can flip at most one 0.
>
> **Example 1:**
>
> ```
> Input: [1,0,1,1,0]
> Output: 4
> Explanation: Flip the first zero will get the the maximum number of consecutive 1s.
>     After flipping, the maximum number of consecutive 1s is 4.
> ```
>
> **Follow up:**
>
> 1. 如果可以 k 次flip 怎么办？
> 2. What if the input numbers come in one by one as an **infinite stream**? In other words, you can't store all numbers coming from the stream as it's too large to hold in memory. Could you solve it efficiently?

### 思路

下面的解法就是拓展到 k 次flip的，其实该题目可以理解为一个最多可以包括k个zero的sliding window，该sliding window的最大长度是多少。所以用2个pointer 滑动，然后一个variable记录当前window中的zero的个数，一直滑动右边的pointer，如果zero个数大于k，那么滑动左边的pointer直到zero个数 &lt;= k。同时更新符合条件的window的最大长度。

### Code

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int max = 0, zero = 0, k = 1;
        int start = 0, end = 0;
        while(end < nums.length) {
            if(nums[end++] == 0) zero ++;
            while(zero > k) {
                if(nums[start++] == 0) zero --;
            }
            max = Math.max(end - start, max);
        }
        return max;
    }
}
```



