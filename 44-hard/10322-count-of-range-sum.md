# 327. Count of Range Sum

> Given an integer array `nums`, return the number of range sums that lie in `[lower, upper]`inclusive.
>
> Range sum `S(i, j)`is defined as the sum of the elements in `nums`between indices `i`and `j`\(`i <= j`\), inclusive.
>
> **Note:**
>
> A naive algorithm of O\(n^2\) is trivial. You MUST do better than that.
>
> **Example:**
>
> Given _nums_ =`[-2, 5, -1]`, _lower _=`-2`,  _upper _=`2`,
>
> Return`3`.
>
> The three ranges are :`[0, 0]`,`[2, 2]`,`[0, 2]`and their respective sums are: `-2, -1, 2`.

### 思路

这道题目难度较大，直接在网上看的大神的帖子。

由于区间\[i, j\]的和等于\[0, j\] - \[0, i-1\]，那么可以先算出所有的前缀和sums\[i\] = \[0, i\]，有利于后面的计算。

这里巧妙的地方在于利用divide and conquer, 即类似与merge sort来处理。想到用merge sort，是因为这样左半边和右半边的区间的sum都是有序的，而且不会破坏左区间和右区间的相对位置，这样可以用two  pointers来计算 \(sums\[j\] - sums\[i\]\)符合要求的个数了。

在merge sort中，需要枚举左边区间的坐标i，找到右区间中：

* 第一个 sums\[l\] - sums\[i\]大于等于lower的临界值坐标 l
* 第一个 sums\[r\] - sums\[i\]大于upper的临界值坐标 r

则对于左边区间的 i 来说，能在右边找到的valid的区间数为\(r - l\)

因为左边区间都是单调递增的，当 i 向右移，l 和 r 也只可能向右移，所以合并时复杂度为O\(N\)

当然计算完成进行归并排序，保证当前sums中的区间内有序。

### 注意

因为数据的范围是int，但是其相加后的结果可能超过int范围，所以sums数组应该是个long\[\] 数组

### 复杂度

O\(N \* log\(N\)\)，和merge sort的复杂度类似

### Code

```java
public class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        // sums的长度为(nums.length+1), sums[i]表示nums长度为i，即nums[0]-nums[i-1]元素之和 
        long[] sums = new long[nums.length + 1];
        for(int i = 1; i <= nums.length; i ++) sums[i] = sums[i-1] + nums[i-1];
        return countByMergeSort(sums, 0, nums.length + 1, lower, upper);
    }

    public int countByMergeSort(long[] sums, int start, int end, int lower, int upper) {
        // 这里end是exclusive的，sums[]是取不到sums[end]值，所以end - start == 1的时候，表示start已经到头了
        // start是inclusive的
        if(end - start <= 1) return 0;
        int mid = (start + end) / 2, count = 0; // count是返回的结果

        count = countByMergeSort(sums, start, mid, lower, upper) + countByMergeSort(sums, mid, end, lower, upper);
        int r = mid, l = mid, k = mid, j = 0;
        long[] cache = new long[end - start]; // cache就是sums[]在[start,end)之间sorted后的结果

        for(int i = start; i < mid; i ++) {
            // 找第一个 sums[l] - sums[i] >= lower 的坐标
            while(l < end && sums[l] - sums[i] < lower) l++;
            // 找第一个 sums[r] - sums[i] > upper 的坐标
            while(r < end && sums[r] - sums[i] <= upper) r++;
            // 这里是对sums的[start, end) 区间进行merge
            while(k < end && sums[k] < sums[i]) cache[j++] = sums[k++];
            cache[j++] = sums[i];
            count += r - l;
        }
        // copy的长度只用是(k - start)即可，不能是(end - start)，因为cache中赋值的长度只有(k-start)
        // k 到 end 这一段可能都是0，所以不能copy
        System.arraycopy(cache, 0, sums, start, k - start);
        return count;
    }
}
```



