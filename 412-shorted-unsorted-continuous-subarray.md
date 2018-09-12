# 581. Shortest Unsorted Continuous Subarray

> Given an integer array, you need to find one **continuous subarray **that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.
>
> You need to find the **shortest **such subarray and output its length.
>
> **Example 1**:
> ```
> Input: [2, 6, 4, 8, 10, 9, 15]
> Output: 5
> Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
> ```

1\) copy 原来的array 到新的array，然后sort 新的array，把sorted的array和原来的array对比，元素位置不同的点便是unsorted subarray 的点。

```java
public class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] sorted = Arrays.copyOf(nums, nums.length);
        Arrays.sort(sorted);
        int min = nums.length;
        int max = -1;
        for (int i = 0; i < nums.length; i ++) {
            if (nums[i] != sorted[i]) {
                min = Math.min(i, min);
                max = Math.max(i, max);
            }
        }
        return max - min + 1 < 0 ? 0 : max - min + 1;
    }
}
```

2\) 不用extra space和sort原来的array。从array开头开始遍历，如果元素i不是ascending order排列的，则i可能是unsorted subarray的end point，注意这里不是begin point, 因为i比之前的subarray中的max小，i之前的subarray中也有需要sorted的。遍历一次后，可以找到unsorted subarray的end。 同理从array 最后一个元素遍历到开头元素，找到unsorted subarray的begin point。

```java
public class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int beg = -1, end = nums.length, min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i ++) {
            max = Math.max(nums[i], max);
            if (nums[i] < max) {
                end = i;
            }
        }
        for (int i = nums.length - 1; i >= 0; i --) {
            min = Math.min(nums[i], min);
            if (nums[i] > min) {
                beg = i;
            }
        }
        // optimization: 上面两个for也可以合并成一个for loop
        return end - beg > nums.length ? 0 : end - beg + 1;
     }
} 
```



