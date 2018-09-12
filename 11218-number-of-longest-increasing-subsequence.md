# 673. Number of Longest Increasing Subsequence

> Given an unsorted array of integers, find the number of longest increasing subsequence.
>
> **Example 1:**
>
> ```
> Input: [1,3,5,4,7]
> Output: 2
> Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
> ```
>
> **Example 2:**
>
> ```
> Input: [2,2,2,2,2]
> Output: 5
> Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
> ```
>
> **Note:**
>
> Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.

### 思路

1. 首先想怎么得到longest increasing subsequence。那么就和 [9.2.1 Longest Increasing Subsequence](/9.2.1)  题目一样，用DP的 “选点拼接” 来解。即用一个数组int\[\] len 来记录 “len\[i\] 表示到 以 i 为结尾的longest increasing subsequence的长度”。
2. 但是这里还要求最长的递增子序列一共有多少个，所以还需要一个数组来储存这个信息。即开另外一个 int\[\] count来记录 “count\[i\] 表示以i为结尾的最长递增子序列的个数”。最后得到整个数组的LIS长度max后，遍历int\[\] count和int\[\] len，把len\[i\] == max的index i 对应的count\[i\] 相加即可。

### Code

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int max = 0, n = nums.length;
        int[] count = new int[n]; // 记录以nums[i]结尾的LIS的个数
        int[] len = new int[n]; // 记录以nums[i] 结尾的LIF的长度
        for(int i = 0; i < n; i ++) {
            count[i] = 1;
            len[i] = 1;
            for(int j = 0; j < i; j ++) {
                if(nums[i] > nums[j]) {
                    if(len[i] == len[j] + 1) count[i] += count[j];
                    else if(len[i] < len[j] + 1) {
                        len[i] = len[j] + 1;
                        count[i] = count[j];
                    }
                }
            }
            max = Math.max(max, len[i]);
        }
        
        int rst = 0;
        for(int i = 0; i < n; i ++) {
            if(len[i] == max) rst += count[i];
        }
        return rst;
    }
}
```



