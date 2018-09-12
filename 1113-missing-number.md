# 268. Missing Number

> Given an array containing n distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.
>
> **Example 1:**
>
> ```
> Input: [3,0,1]
> Output: 2
> ```
>
> **Example 2:**
>
> ```
> Input: [9,6,4,2,3,5,7,0,1]
> Output: 8
> ```
>
> **Note:**
>
> Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

### 思路

这里需要linear time 和 constant space。因为已经知道从 0 到 n 这 \(n+1\) 中取了n个不同的数，那么容易求得该 n+1 个数的和，减去nums\[\] 中所有元素的和，便是 缺少的那个数字。

### Code

```java
class Solution {
    public int missingNumber(int[] nums) {
        int sum = (nums.length) * (nums.length + 1) / 2;
        for(int num : nums) sum -= num;
        return sum;
    }
}
```





