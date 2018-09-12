# 674. Longest Continuous Increasing Subsequence

> Given an unsorted array of integers, find the length of longest `continuous`increasing subsequence \(subarray\).
>
> **Example 1:**
>
> ```
> Input: [1,3,5,4,7]
> Output: 3
> Explanation: The longest continuous increasing subsequence is [1,3,5], its length is 3. 
> Even though [1,3,5,7] is also an increasing subsequence, it's not a continuous one where 5 and 7 are separated by 4.
> ```
>
> **Example 2:**
>
> ```
> Input: [2,2,2,2,2]
> Output: 1
> Explanation: The longest continuous increasing subsequence is [2], its length is 1.
> ```
>
> **Note:**
>
> Length of the array will not exceed 10,000.

### 思路

1. 这里是找一个单调增长的区间，即需要维护一个单调增长的window。window的题目 最容易想到“双针模型”，用一个start，end pointer来维护一个单调增长的区间即可。
2. 一开始start，end 起始点为0，即最左边。然后end处元素和下一个相互比较，如果下一个元素比end大，则说明是单调增长的，end ++ 即可，直到该单调区间结束为止。然后记录该当前单调区间的长度。然后开始找下一个单调区间，如此直到scan完该数组。

### Code

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int start = 0, end = 0, rst = 0;
        while(end < nums.length) {
            while(end < nums.length - 1 && nums[end] < nums[end + 1]) end ++;
            rst = Math.max(rst, end - start + 1);
            end ++; // end ++ 表示开始找下一个单调区间，肯定是从当前单调区间end的下一个元素开始的
            start = end;
        }
        return rst;
    }
}
```



