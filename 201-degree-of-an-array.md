# 697. Degree of an Array

> Given a non-empty array of non-negative integers`nums`, the **degree **of this array is defined as the maximum frequency of any one of its elements.
>
> Your task is to find the smallest possible length of a \(contiguous\) subarray of`nums`, that has the same degree as`nums`.
>
> **Example 1:**
>
> ```
> Input: [1, 2, 2, 3, 1]
> Output: 2
> Explanation: 
> The input array has a degree of 2 because both elements 1 and 2 appear twice.
> Of the subarrays that have the same degree:
> [1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
> The shortest length is 2. So return 2.
> ```
>
> **Example 2:**
>
> ```
> Input: [1,2,2,3,1,4,2]
> Output: 6
> ```
>
> **Note:**
>
> `nums.length`will be between 1 and 50,000.
>
> `nums[i]`will be an integer between 0 and 49,999.

### 思路

1. 一开始想到扫一遍来记录数字出现的frequency，然后来求得原数组的degree。如果只有一个元素出现的frequency是degree，那么就用该元素出现的\(most right index - most left index + 1\) 便是所求答案，因为subarray必须包含所有的该元素。
2. 但是有可能有好几个元素\(a, b, c\)出现的frequency都等于degree，那么就需要看那几个分别包含所有\(a, b, c\)的subarray的长度，取最小长度即可。

### Code

```java
class Solution {
    public int findShortestSubArray(int[] nums) {
        HashMap<Integer, Integer> count = new HashMap(); // 记录元素出现的次数
        HashMap<Integer, Integer> minIndex = new HashMap(); // 记录每个元素出现的most left index
        HashMap<Integer, Integer> maxIndex = new HashMap(); // 记录每个元素出现的most right index
        int degree = 0;
        for(int i = 0; i < nums.length; i ++) {
            count.put(nums[i], count.getOrDefault(nums[i], 0) + 1);
            degree = Math.max(degree, count.get(nums[i]));
            if(!minIndex.containsKey(nums[i])) minIndex.put(nums[i], i);
            maxIndex.put(nums[i], i);
        }
        
        int rst = nums.length;
        for(int key: count.keySet()) {
            if (degree == count.get(key)) {
                rst = Math.min(rst, maxIndex.get(key) - minIndex.get(key) + 1);
            }
        }
        return rst;
    }
}
```



