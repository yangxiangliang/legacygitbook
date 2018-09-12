# 34. Find First and Last Position of Element in Sorted Array

> Given an array of integers `nums`sorted in ascending order, find the starting and ending position of a given `target`value.
>
> Your algorithm's runtime complexity must be in the order of _O_\(log\(_n\)_\).
>
> If the target is not found in the array, return`[-1, -1]`.
>
> **Example 1:**
>
> ```
> Input: nums = [5,7,7,8,8,10], target = 8
> Output: [3,4]
> ```
>
> **Example 2:**
>
> ```
> Input: nums = [5,7,7,8,8,10], target = 6
> Output: [-1,-1]
> ```

### Code

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int start = binarySearch(nums, target);
        // 注意 nums 中其实没有target的情况
        if(start == nums.length || nums[start] != target) return new int[]{-1, -1};
        // 找(target + 1)在nums中的位置是巧妙之处
        return new int[]{start, binarySearch(nums, target + 1) - 1};
    }

    // 该函数返回的是：1）如果nums中有target，则是第一个出现的target的位置；
    // 2) 如果nums中没有target，则是target应该插入nums中的位置
    private int binarySearch(int[] nums, int target) {
        int start = 0, end = nums.length - 1;

        while(start <= end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] < target) start = mid + 1;
            else if (nums[mid] == target) {
                // 检查 mid 是不是target第一次出现的位置
                if(mid == start || nums[mid] != nums[mid - 1]) return mid;
                end = mid - 1;
            } else end = mid - 1;
        }
        return start;
    }
}
```



