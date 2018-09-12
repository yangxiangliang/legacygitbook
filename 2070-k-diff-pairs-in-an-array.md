# 532. K-diff Pairs in an Array

> Given an array of integers and an integer **k**, you need to find the number of **unique **k-diff pairs in the array. Here a **k-diff **pair is defined as an integer pair \(i, j\), where **i **and **j **are both numbers in the array and their [absolute difference](https://en.wikipedia.org/wiki/Absolute_difference) is **k**.
>
> **Example 1:**
>
> ```
> Input: [3, 1, 4, 1, 5], k = 2
> Output: 2
> Explanation: There are two 2-diff pairs in the array, (1, 3) and (3, 5).
> Although we have two 1s in the input, we should only return the number of unique pairs.
> ```
>
> **Example 2:**
>
> ```
> Input:[1, 2, 3, 4, 5], k = 1
> Output: 4
> Explanation: There are four 1-diff pairs in the array, (1, 2), (2, 3), (3, 4) and (4, 5).
> ```
>
> **Example 3:**
>
> ```
> Input: [1, 3, 1, 5, 4], k = 0
> Output: 1
> Explanation: There is one 0-diff pair in the array, (1, 1).
> ```
>
> **Note:**
>
> 1. The pairs \(i, j\) and \(j, i\) count as the same pair.

### 思路

1. 先把array sort一下，维护一个sliding window，window最右边的值和最左边的值相差 k 即可。需要注意的就是需要unique pair，那么在移动sliding window的时候，之前visit过的相同的值需要skip掉即可。

### Code

```java
class Solution {
    public int findPairs(int[] nums, int k) {
        Arrays.sort(nums);
        int start = 0, end = 1, count = 0;

        while(end < nums.length) {
            while(end < nums.length && nums[end] - nums[start] < k) end ++;
            if(end == nums.length) break;

            if(nums[end] - nums[start] == k) count ++;

            while(start < nums.length - 1 && nums[start] == nums[start + 1]) start ++;
            start ++;

            while(end <= start) end ++;
        }
        return count;
    }
}
```



