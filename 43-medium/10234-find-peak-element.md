# 162. Find Peak Element

> A peak element is an element that is greater than its neighbors.
>
> Given an input array where`num[i] ≠ num[i+1]`, find a peak element and return its index.
>
> The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.
>
> You may imagine that`num[-1] = num[n] = -∞`.
>
> For example, in array`[1, 2, 3, 1]`, 3 is a peak element and your function should return the index number 2.
>
> **Note:**
>
> Your solution should be in logarithmic complexity.

### 思路

由于这里要求time complexity是log\(N\)，所以不能用brute force的方法遍历array。在array中找到log\(N\)的方法，容易想到binary search的思想。此题就是相当于寻找 **local maximum** 的题目，因为可以认为 num\[-1\] 和 num\[n\] 都等于无穷小，所以num\[i\] &lt; num\[i+1\]，那么\[i+1，...., n -1 \]一定有local maximum；如果 num\[i\] &gt; num\[i+1\]，那么\[0, 1, 2..... i\]一定存在local maximum，用binary search即可查找。

### Code

```java
public class Solution {
    public int findPeakElement(int[] nums) {
        int start = 0;
        int end = nums.length - 1;
        while(start < end) {
            int mid = start + (end - start) / 2;
            if(nums[mid+1] > nums[mid]) start = mid + 1;
            else end = mid;
        }
        return start;
    }
}
```



