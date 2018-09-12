# 153. Find Minimum in Rotated Sorted Array

> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
>
> Find the minimum element.
>
> **You may assume no duplicate exists in the array.**

### Code

```java
class Solution {
    public int findMin(int[] nums) {
        int rst = Integer.MAX_VALUE;
        int start = 0, end = nums.length - 1;

        while(start <= end) {
            int mid = start + (end - start) / 2;

            // 判断 mid, start是否在rotated后的“同一侧递增序列”
            if(nums[mid] >= nums[start]) {
                rst = Math.min(rst, nums[start]);
                start = mid + 1;
            } else {
                rst = Math.min(rst, nums[mid]);
                end = mid - 1;
            }
        }
        return rst;
    }
}
```

### Follow up: There are duplicates in the array

```java
class Solution {
    public int findMin(int[] nums) {
        int rst = Integer.MAX_VALUE;
        int start = 0, end = nums.length - 1;

        while(start <= end) {
            int mid = start + (end - start) / 2;

            // 此时 nums[mid] == nums[start]时，无法判断 mid, start是否在rotated后的“同一侧递增序列”
            if(nums[mid] > nums[start]) {
                rst = Math.min(rst, nums[start]);
                start = mid + 1;
            } else if(nums[mid] < nums[start]) {
                rst = Math.min(rst, nums[mid]);
                end = mid - 1;
            // nums[mid] == nums[start]时，把nums[start]和rst比较，然后start++
            } else rst = Math.min(rst, nums[start++]);
        }
        return rst;
    }
}
```



