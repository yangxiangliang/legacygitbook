# 801. Minimum Swaps To Make Sequences Increasing

> We have two integer sequences`A`and`B`of the same non-zero length.
>
> We are allowed to swap elements`A[i]`and`B[i]`.  Note that both elements are in the same index position in their respective sequences.
>
> At the end of some number of swaps,`A`and`B`are both strictly increasing.  \(A sequence is strictly increasing if and only if`A[0] < A[1] < A[2] < ... < A[A.length - 1]`.\)
>
> Given A and B, return the minimum number of swaps to make both sequences strictly increasing.  It is guaranteed that the given input always makes it possible.
>
> ```
> Example:
> Input: A = [1,3,5,4], B = [1,2,3,7]
> Output: 1
> Explanation: 
> Swap A[3] and B[3].  Then the sequences are:
> A = [1, 3, 5, 7] and B = [1, 2, 3, 4]
> ```
>
> **Note:**
>
> * `A, B`are arrays with the same length, and that length will be in the range \[1, 1000\].
>
> * `A[i], B[i]`are integer values in the range`[0, 2000].`

### 思路

1. 这里需要最小的swap次数。显然 A\[i\], B\[i\] 是否swap是和A\[i-1\], B\[i-1\]的相对大小，和A\[i-1\], B\[i-1\] 处是否已经swap相关的。因此每个对应的swap A\[i\], B\[i\] 都有2种状态，即i对应的A, B数组里面的元素是否swap。因为到数组 i 处swap的最小次数和 \(i-1\) 处swap次数以及\(i-1\) 处是否swap相关，容易想到用DP来记录这些状态。因为每个i处都有swap与否2种状态，所以需要用2个数组int\[\] swap, int\[\] noSwap 记录。swap\[i\] 表示i处A, B对应元素swap时，从头到i处需要的最小swap数。noSwap\[i\] 表示 i 处A, B 对应元素不 swap时，从头到i处需要的最小swap的数量。
2. 然后在DP的时候，需要根据A\[i\], A\[i-1\], B\[i\], B\[i-1\]的大小情况，来判断 i 处是否需要swap。

### Code

```java
class Solution {
    public int minSwap(int[] A, int[] B) {
        int n = A.length;
        int[] no_swap = new int[n];
        int[] swap = new int[n];
        swap[0] = 1;
        for(int i = 1; i < A.length; i ++) {
            // 这种情况本来i和（i-1）处本来都可以不需要swap
            if(A[i] > A[i-1] && B[i] > B[i-1]) {
                // 表示即便i-1 处swap掉，i处也可以不swap
                if(A[i] > B[i-1] && B[i] > A[i-1]) {
                    no_swap[i] = Math.min(no_swap[i-1], swap[i-1]);
                    swap[i] = Math.min(no_swap[i-1], swap[i-1]) + 1; // 1表示i处swap的操作
                }
                else {
                    // 这种情况下， 表示i，(i-1) 必须都swap，或则都不swap才能保证递增序列
                    no_swap[i] = no_swap[i-1];
                    swap[i] = swap[i-1] + 1;
                }
            } else {
            // 因为i, (i-1) 处本来不满足递增条件，所以必须有1个swap，一个2 swap才行，得出下面递推关系
                no_swap[i] = swap[i-1];
                swap[i] = no_swap[i-1] + 1;
            }
        }
        return Math.min(swap[n-1], no_swap[n-1]);
    }
}
```



