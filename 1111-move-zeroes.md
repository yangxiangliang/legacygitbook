# 283. Move Zeroes

> Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.
>
> For example, given`nums = [0, 1, 0, 3, 12]`, after calling your function,`nums`should be`[1, 3, 12, 0, 0]`.
>
> **Note:**
>
> 1. You must do this **in-place** without making a copy of the array.
> 2. Minimize the total number of operations.

### 思路

1. 看见题目就想到，先找到最开始的0，然后和后面的non-zero integer 交换即可。比如题目中\[0, 1, 0, 3, 12\]，一开始0和1交换了，变成\[1, 0, 0, 3, 12\]，显然应该3和第一个0交换，得到\[1, 3, 0, 0, 12\]，然后12和第一个0交换即可。
2. 则想到维护2个pointer，一个pointer zero 指向array中第一个0的index，另一个pointer i 指向 zero后的第一个non-zero元素，然后交换他们即可。直到第二个指针指向array的末尾，表示交换完毕。关键在维护第一个pointer时，怎么保证一直是first 0。
   1. zero pointer 的元素和 i pointer元素swap后，如果i == zero + 1，那么zero 指针加1便是swap后array中的first 0
   2. 如果 i &gt; zero + 1，说明i 和 zero之间还有其他0，则swap\(zero, i\) 后，还是zero++，便表示此刻array中的first 0

### Code

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int zero = 0; // index for first zero in current array
        while(zero < nums.length && nums[zero] != 0) zero ++; // find initial index for first 0

        // Note: i应该从第一个zero开始，因为第一个zero之前的non-zero integer不需要变动位置，不用关心
        for(int i = zero; i < nums.length; i ++) {
            if (nums[i] != 0) {
                swap(nums, zero, i);
                // Note: zero ++ 便是swap后的array中第一个0的index
                zero++;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

### Code 2）

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int pos = 0;
        for(int num : nums) {
            if(num != 0) nums[pos++] = num;
        }
        
        while(pos < nums.length) nums[pos++] = 0;
    }
}
```



