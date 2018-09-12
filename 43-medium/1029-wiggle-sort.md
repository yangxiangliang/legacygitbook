# 280. Wiggle Sort

> Given an unsorted array `nums`, reorder it **in-place **such that `nums[0] <= nums[1] >= nums[2] <= nums[3]...`.
>
> For example, given `nums = [3, 5, 2, 1, 6, 4]`, one possible answer is `[1, 6, 2, 5, 3, 4]`.

### 思路

观察数组，从index为0开始，如果nums\[0\] &gt; nums\[1\], 那么swap 0，1即可此时nums\[0\] &lt; nums\[1\]。然后我们需要nums\[1\] &gt;= nums\[2\]，如果此时nums\[1\] &lt; nums\[2\]，那么需要swap1，2，因为之前的nums\[1\] 已经比nums\[0\]大了，此时nums\[2\]比nums\[1\]更大，交换1，2后，nums\[1\]肯定也比nums\[0\]和nums\[2\]大，所以符合要求。依次方法类推即可。**注意:** 如果 i % 2 == 0，需要检查是不是nums\[i\] &gt; nums\[i+1\]，如果是，则需要swap他们；如果 i % 2 == 1，则检查是否nums\[i\] &lt; nums\[i+1\]，如果是，则swap他们。

### Code

```java
public class Solution {
    public void wiggleSort(int[] nums) {
        for(int i = 0; i < nums.length - 1; i ++) {
            if(i % 2 == 0) {
                if(nums[i] > nums[i+1]) swap(nums, i, i + 1);
            } else if(nums[i] < nums[i+1]) swap(nums, i, i + 1);
        }
    }

    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```



