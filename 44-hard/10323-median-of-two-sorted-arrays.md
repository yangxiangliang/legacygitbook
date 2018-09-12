# 4. Median of Two Sorted Arrays

> There are two sorted arrays **nums1 **and **nums2 **of size m and n respectively.
>
> Find the median of the two sorted arrays. The overall run time complexity should be O\(log \(m+n\)\).
>
> **Example 1:**
>
> ```
> nums1 = [1, 3]
> nums2 = [2]
>
> The median is 2.0
> ```
>
> **Example 2:**
>
> ```
> nums1 = [1, 2]
> nums2 = [3, 4]
>
> The median is (2 + 3)/2 = 2.5
> ```

### 思路

看见题目要求complexity是log\(m+n\)，而且nums1和nums2都是sorted，容易想到用binary search。

**此题延伸一下，可以转化为求2个sorted的array中第K大的element的值**。

递归处理：分别计算nums1和nums2的第k/2个数，并比较两者的大小。如果nums1的第k/2个数小于nums2的第k/2个数，那么结果不可能出现在nums1中的前k/2个数中；此时可以减掉nums1中的前k/2个数，并进入下一次递归；如果nums1的第k/2个数大于nums2中的第k/2个数，那么结果不可能出现在nums2中的前k/2个数中，此时可以减掉nums2中的前k/2个数，并进入下一次递归。

### 注意

此题中 **findKth** function 也可以延伸用到题目：找2个sorted array中第K大的元素。

### Code

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int length = nums1.length + nums2.length;
        if(length % 2 == 1) return findKth(nums1, 0, nums2, 0, (length+1)/2);
        return (double)(findKth(nums1, 0, nums2, 0, length / 2) + findKth(nums1, 0, nums2, 0, length/2 + 1)) / 2;
    }
    
    public int findKth(int[] a, int aStart, int[] b, int bStart, int k) {
        // 判断边界情况
        if(aStart >= a.length) return b[bStart+k-1];
        if(bStart >= b.length) return a[aStart+k-1];
        if(k == 1) return Math.min(a[aStart], b[bStart]);

        // aKey, bKey初始化都为Integer.MAX_VALUE
        // 比如aStart + k/2 - 1 >= a.length时，那么说明结果不可能出现在b的前k/2中
        // 因为a中已经没有k/2个元素了，就算把a中剩下的元素都比b中的小，b中的第k/2个元素也不可能是第k大的元素
        // 所以相当于舍弃b的前k/2元素，相当于把aKey的值看作MAX_VALUE       
        int aKey = Integer.MAX_VALUE, bKey = Integer.MAX_VALUE;
        if(aStart + k/2 - 1 < a.length) aKey = a[aStart + k/2 - 1];
        if(bStart + k/2 - 1 < b.length) bKey = b[bStart + k/2 - 1];
        if(aKey < bKey) return findKth(a, aStart + k/2, b, bStart, k - k/2);
        else return findKth(a, aStart, b, bStart + k/2, k - k/2);
    }
}
```



