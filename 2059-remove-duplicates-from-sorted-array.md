# 26. Remove Duplicates from Sorted Array

> Given a sorted array _nums_, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only _once_ and return the new length.
>
> Do not allocate extra space for another array, you must do this by **modifying the input array **[**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) with O\(1\) extra memory.
>
> **Example 1:**
>
> ```
> Given nums = [1,1,2],
>
> Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
>
> It doesn't matter what you leave beyond the returned length.
> ```
>
> **Example 2:**
>
> ```
> Given nums = [0,0,1,1,1,2,2,3,3,4],
>
> Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.
>
> It doesn't matter what values are set beyond the returned length.
> ```

### 思路

1. 这里需要in-place的处理，把2个pointer，一个pointer用来遍历，一个pointer指向当前不重复的integer应该放的位置。那么遍历integer的时候看是否和之前遍历过的integer有重复，不重复的话则放入第二个pointer的位置。第二个pointer最后的位置也就是不重复的元素的个数。

### Code

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length;
        if(len < 2) return len;

        int size = 1;
        for(int i = 1; i < len; i ++) {
            if(nums[size - 1] != nums[i]) {
                nums[size++] = nums[i];
            }
        }
        return size;
    }
}
```



