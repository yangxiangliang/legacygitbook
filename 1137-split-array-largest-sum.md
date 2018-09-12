# 410. Split Array Largest Sum

> Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.
>
> **Note:**
>
> If n is the length of array, assume the following constraints are satisfied:
>
> * 1 &lt;= n &lt;= 1000
>
> * 1 &lt;= m &lt;= min\(50, n\)
>
> **Example:**
>
> ```
> Input:
> nums = [7,2,5,10,8]
> m = 2
>
> Output:
> 18
>
> Explanation:
> There are four ways to split nums into two subarrays.
> The best way is to split it into [7,2,5] and [10,8],
> where the largest sum among the two subarrays is only 18.
> ```

### 思路

1. 一开始拿到题目没什么思路，就想怎么才能保证最后分割的 m subarrays 的和的最大值最小。假设数组nums的总的和是sum，肯定需要尽量均匀的分配sum到m个subarray，这样才符合要求。最好m个subarray的和越接近越好。但按照这个思路，继续往下想，便没什么思路了。
2. 想怎么才能找到m个subarray的和的最大值 max？这个最大值有什么条件和范围吗？首先这个最大值肯定小于等于整个nums的和sum，然后其肯定大于等于nums中元素nums\[i\]的最大值。因为不管怎么分割，至少有1个元素在subarray里。**这里便出现了在这个left bound, right bound 区间里找到最小的该max值即可。在某个空间bounds里面找合适的元素，最容易想到efficient的方法就是binary search，那么接着这个思路往下，看能不能用binary search来寻找。**
3. 首先想如果找到一个max的值 x，我们怎么才能判断其是否是有效的，这样才能决定binary search的下一步的操作。这里因为subarray是连续的，其实很容易判断，从数组nums 头开始，记录subarray的和，如果当前subarray的和超过了 x，那么现在这个nums\[i\] 肯定是下一个subarray里的，用一个counter记录，加1即可。这样用最greedy的方法，scan结束后，看counter 值 与 m值的比较。
   1. 如果counter 比 m大，说明用 x 值，无法划分m 个subarray，让其值都不超过x。那么 x 需要取更大的值。
   2. 如果 counter 小于等于 m，说明 x 值有效，那么看有没有办法再找到更小的 x 值同样符合要求。那么 x 在binary search中取更小 的值再尝试。

### 复杂度

Time: 因为binary search 的复杂度是log\(N\), N = nums的和 - nums中最大的元素。每一次binary search中，判断一个点是否valid的复杂度是O\(n\)，n = nums的长度。所以总的复杂度是 O\(log\(nums的和 - nums中最大的元素\) \* nums.length\)

### Code

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        long sum = 0;
        int max = 0;
        for(int num : nums) {
            sum += num;
            max = Math.max(max, num);
        }
        if(m == 1) return (int) sum;
        long start = max, end = sum;
        while(start <= end) {
            long mid = (start + end) / 2;
            if(valid(mid, nums, m)) end = mid - 1;
            else start = mid + 1;
        }
        // 注意这里return的是start，而不是end。可以直接想终止的条件，比如start == end的时候进去while循环
        // 此时mid = start = end，如果是valid，那么end = mid - 1，退出while循环，mid - 1不是valid的。
        // 所以不能return end。如果不是valid，那么说明mid需要更大，start = mid + 1 退出while循环，也应该return start。
        return (int) start;
    }

    private boolean valid(long target, int[] nums, int m) {
        long sum = 0;
        // count 初始值为1，而不是0，因为至少nums被划分成1个subarray
        int count = 1;
        for(int num : nums) {
            sum += num;
            if(sum > target) {
                count ++;
                sum = num;
            }
            if(count > m) return false;
        }
        return true;
    }
}
```



