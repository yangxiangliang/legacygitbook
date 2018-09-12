# 493. Reverse Pairs

> Given an array `nums`, we call`(i, j)`an **important reverse pair **if `i < j`and `nums[i] > 2*nums[j]`.
>
> You need to return the number of important reverse pairs in the given array.
>
> **Example 1:**
>
> ```
> Input: [1,3,2,3,1]
> Output: 2
> ```
>
> **Example 2:**
>
> ```
> Input: [2,4,3,5,1]
> Output: 3
> ```

### 思路

1. 这里O\(N^2\) 的方法是brute force，显然需要更好的方法。因为这里需要寻找 i &lt; j 的部分多少 nums\[i\] &gt; 2 \* nums\[j\]，那么想到可不可以把原来的数组分为2部分，左边部分的index都是小于走边部分的index，然后再来找复合条件的pair。这样就是 devide and conqure 的思路。
2. 因为把原来的数组分为2部分分别来处理，容易想到merge sort的感觉。如果merge sort的话，左边一部分是sorted的，右边一部分也是sorted的，而且左边的index 永远小于右边的index。那么寻找 reverse pair，就只用从左边部分的start index开始，看右边部分的有多少数字满足条件，则加入结果中即可。

### 复杂度

Time: O\(N \* log\(N\)\)

### Code

```java
class Solution {
    int[] helper; // 在 merge的时候，帮助store orignal的元素，因为merge步骤的时候会update nums[] 中的元素
    public int reversePairs(int[] nums) {
        helper = new int[nums.length];
        return mergeSort(nums, 0, nums.length - 1);
    }

    private int mergeSort(int[] nums, int start, int end) {
        if(start >= end) return 0; // 边界条件不能少
        int mid = start + (end - start) / 2;
        int count = mergeSort(nums, start, mid) + mergeSort(nums, mid + 1, end);

        int j = mid + 1;
        for(int i = start; i <= mid; i ++) {
           // 寻找[mid + 1, end]中有多少满足 nums[i] / 2.0 > nums[j]
           // 因为其是sorted，所以如果j是第一个 >= nums[i] / 2.0的，那么满足的个数是 j - (mid + 1)
           // 这里用 nums[i] / 2.0 来比较，而不用 nums[i] > 2 * nums[j] 是为了防止 overflow
            while(j <= end && nums[i] / 2.0 > nums[j]) j ++;
            count += j - mid - 1;
        }
        merge(nums, start, mid, end);
        return count;
    }

    private void merge(int[] nums, int start, int mid, int end) {
        //注意: 现在merge区域的值赋给helper，因为后面merge的时候会update nums[]中original的值
        for (int i = start; i <= end; i ++) helper[i] = nums[i];

        int i = start, j = mid + 1;
        int index = start;
        while(i <= mid || j <= end) {
            if(i <= mid && j <= end && helper[i] < helper[j] || j > end) {
                 nums[index++] = helper[i++];
            } else nums[index++] = helper[j++];
        }
    }
}
```



