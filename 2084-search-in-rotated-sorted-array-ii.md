# 81. Search in Rotated Sorted Array II

> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
>
> You are given a target value to search. If found in the array return`true`, otherwise return`false`.
>
> **Note:** Input array may contain duplicates.

### 思路

和 search in rotated sorted array 类似，只是这里有可能有duplicate。

### Code

```java
public class Solution {
    public boolean search(int[] nums, int target) {
        if(nums.length == 0) return false;
        int start = 0, end = nums.length - 1;

        // 每一次，start, end 距离至少减少1，所以这里可以取 等号 "="
        while(start <= end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] == target) return true;

            // 判断 mid, start是不是在同一段递增序列里
            if(nums[mid] > nums[start]) {
                if(target >= nums[start] && target < nums[mid]) end = mid - 1;
                else start = mid + 1;
            } else if (nums[mid] < nums[start]) {
                if(target <= nums[end] && target > nums[mid]) start = mid + 1;
                else end = mid - 1;
            } else start ++;
        }
        return false;
    }
}
```



