# 324. Wiggle Sort II

> Given an unsorted array`nums`, reorder it such that`nums[0] < nums[1] > nums[2] < nums[3]...`.
>
> **Example:**
>
> \(1\) Given `nums = [1, 5, 1, 1, 6, 4]`, one possible answer is `[1, 4, 1, 5, 1, 6]`.
>
> \(2\) Given `nums = [1, 3, 2, 2, 3, 1]`, one possible answer is`[2, 3, 1, 3, 1, 2]`.
>
> **Note: **
>
> You may assume all input has valid answer.
>
> **Follow up:**
>
> Can you do it in O\(n\) time and/or in-place with O\(1\) extra space?

### 思路 

这个题目与 题目[10.2.9](/43-medium/1029-wiggle-sort.md) 类似，但是这里相邻的2个元素不能相等，所以不能像 10.2.9 一样，直接比较相邻的2个元素大小来swap，需要处理相等元素相邻的情况。最容易想到的方法就是将元素组copy到一个新的数组，让后将新的数组sort，然后从sort的数组中把元素放回原数组中。

* 因为index为偶数位应该尽量放小的元素，奇数位放大的元素，所以一开始想到从sorted array的一头一尾开始，头元素放入偶数位，尾元素放入奇数位，然后维护这2个pointer同时往sorted array中间靠拢。**这个方法有问题，因为head, end 指针往中间靠拢时，最后head + 1 == end的时候，这2个元素可能相等，而且放入原数组的位置相邻，不符合要求。**e.g. 1, 1, 2, 2, 3, 3，最后head, end pointers都会分别指向2，2，两个2 会相邻的放在一起。
* 所以可以改变方法，找到中间元素的指针 mid，将其放入偶数index处，然后mid - 1。尾指针end放入奇数指针处，然后 end -1，这样就可以避免2个相等元素相邻的情况。

### Code

```java
public class Solution {
    public void wiggleSort(int[] nums) {
        int[] sorted = new int[nums.length];
        System.arraycopy(nums, 0, sorted, 0, nums.length);
        Arrays.sort(sorted);
        int mid = (nums.length - 1) / 2 ;
        int end = nums.length - 1;
        for(int i = 0; i < nums.length; i ++) {
            nums[i] = (i & 1) == 1 ? sorted[end--] : sorted[mid--];
        }
    }
}
```

**这里的complexity显然不符合 follow up的要求，没有理解 follow up怎么处理。。。**





