# 31. Next Permutation

> Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.
>
> If such arrangement is not possible, it must rearrange it as the lowest possible order \(ie, sorted in ascending order\).
>
> The replacement must be in-place, do not allocate extra memory.
>
> Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.  
> `1,2,3`→`1,3,2`  
> `3,2,1`→`1,2,3`  
> `1,1,5`→`1,5,1`

### 思路

因为这里要求in-place，那么就要找到哪些位置的integer需要交换来组成next permutation。因为next permutation是比current 大的排列中最小的那一个\(除非next permutation不存在的情况\)。所以需要从右边，即less significant bit来查找，从右边往左边遍历，比如对于nums\[i\]，如果在nums\[i\]的右边，有比nums\[i\]大的数，那么找到比其大的数中最小的那个，与nums\[i\] 交换位置即可。如果从右往左遍历时，数列一直是ascending的，那么说明每个元素的右边没有比其大的integer，那么只能接着continue往左遍历，极端情况就是\[3, 2, 1\]的情况，数列从右到左整个是ascending的，这种情况下，reverse整个数列即可。

### 注意

如果nums\[i\]右边找到比nums\[i\]大的最小数是nums\[j\]，那么交换i, j 后。对于\[i+1, nums.length\) 的元素，需要将其排列成最小，这样才能让最后总的array 排列 结果是最小值。因为nums\[j\]是比nums\[i\]大的最小值，那么\[j+1, nums.length\) 比nums\[i\]小，\[i+1, j-1\]比nums\[i\]大，所以 i 和 j 交换后，此时\[i+1, nums.length\)任然是降序排列，只需要将\[i+1, nums.length\) 一段reverse即可。

### Code

```java
public class Solution {
    public void nextPermutation(int[] nums) {
        for(int i = nums.length - 1; i > 0; i --) {
            if(nums[i] <= nums[i-1]) continue;

            for(int j = nums.length - 1; j > i - 1; j --) {
                if(nums[j] > nums[i-1]) {
                    swap(nums, i-1, j);

                    // sort [i, nums.length - 1] to ascending order
                    reverse(nums, i, nums.length - 1);
                    return;
                }
            }
        }

        // change whole array from descending order to ascending order
        reverse(nums, 0, nums.length - 1);
    }

    // reverse order between array [start, end]
    public void reverse(int[] nums, int start, int end) {
        while(start < end) swap(nums, start ++, end --);
    }

    public void swap(int[] nums, int i, int j) {
        int tmp = nums[j];
        nums[j] = nums[i];
        nums[i] = tmp;
    }
}
```



