# 34. Search for a Range

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

### 思路

1. 因为题目是sorted，而且要求log\(n\) 时间，自然想到 binary search来处理。
2. **这里的想法很巧妙，基本思路就是先找到target的起始点，那么在binary search中要”尽量往前面找target“。每次                 target &lt;= nums\[mid\] 的时候，end = mid。在找target在nums\[\] 中的最后的index的时候，可以找\(target+1\)在nums\[\] 中的index，如果\(target+1\)不存在，即找大于target的最小的数在nums\[\] 中的index，该 index - 1 便是需要的target的末尾的index。所以下面的binary search相当于是找 等于target或则大于target的最小数的index值。**

### Code

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int start = findGreaterEqual(nums, target);
        if(start == nums.length || nums[start] != target) {
            return new int[]{-1, -1};
        }
        // 找出等于或则大于(target+1)的最小index - 1，便是target的最大index值
        return new int[]{start, findGreaterEqual(nums, target + 1) - 1};
    }

    // 如果target存在，找出的target的最小的index。如果不存在，找出的index是大于target的最小数的第一个index
    private int findGreaterEqual(int[] nums, int target) {
        int start = 0, end = nums.length;
        while(start < end) {
            // start <= mid < end
            int mid = (start + end) / 2;
            if(nums[mid] < target) start = mid + 1;
            // 1. nums[mid] > target时候， end 不为 mid - 1，因为这里找第一个大于target或则等于的，可能就是mid，所以 end = mid
            // 2. nums[mid] == target 时候，end也可以 update为mid
            // start == end 时候会break 循环，start = end - 1的时候，也保证每次范围会shrink by 1，不会出现死循环
            else end = mid;
        }
        return start;
    }
}
```



