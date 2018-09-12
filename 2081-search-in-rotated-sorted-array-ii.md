# 81. Search in Rotated Sorted Array II

> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
>
> You are given a target value to search. If found in the array return `true`, otherwise return `false`.
>
> **Note: arrays may contain duplicates**

### 思路

这个题目就是 [Search in Rotated Sorted Array](/2014-search-in-rotated-sorted-array.md) 的 follow up，主要问题是有duplicate。那么在原来题目 nums\[mid\] 和 nums\[start\] 比较的时候，如果nums\[mid\] == nums\[start\]的时候，无法判断mid 和 start是在rotated的“同一侧递增的”，还是“不同一侧”。比如

\[1, 3, 1, 1, 1\]的情况。所以遇见该情况的时候，只能start ++ 缩小范围。

### Code

```java
public class Solution {
    public boolean search(int[] nums, int target) {
        if(nums.length == 0) return false;
        int start = 0, end = nums.length - 1;

        while(start < end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] == target) return true;

            if(nums[mid] > nums[start]) {
                if(target >= nums[start] && target < nums[mid]) end = mid - 1;
                else start = mid + 1;
            } else if (nums[mid] < nums[start]) {
                if(target <= nums[end] && target > nums[mid]) start = mid + 1;
                else end = mid - 1;
            // nums[mid] == nums[start]时候，无法判断mid, start在rotated的同一侧，还是不同一侧
            // 所以只能 start ++ 来缩小范围
            } else start ++;
        }
        return nums[start] == target;
    }
}
```



