# 209. Minimum Size Subarray Sum

> Given an array of **n **positive integers and a positive integer **s**, find the minimal length of a **contiguous **subarray of which the sum ≥ **s**. If there isn't one, return 0 instead.
>
> For example, given the array`[2,3,1,2,4,3]`and`s = 7`,  
> the subarray`[4,3]`has the minimal length under the problem constraint.

### 思路 1\)

1. 最普通的思路，还是用2个for循环，计算 nums\[i\] 到 nums\[j\] 的和，为了简便计算，可以预处理得到int\[\] sums 数组，sums\[i\]的值表示 nums\[0\] 到 nums\[i\] 之和。然后 \(nums\[i\] 到 nums\[j\] 的 和\) = sums\[j\] - sums\[i-1\]
2. 只不过计算的过程中有些可以优化的地方：
   1. 如果 nums\[i\]到nums\[j\] 已经小于 s了，就在该区间里不用再缩小范围了，因为都是positive integer，越缩小范围，得到的结果越小，不可能满足 &gt;= k 的条件。
   2. 如果 结果 rst 长度已经有值了，那么从 nums\[i\] 开始只用遍历到 \(i + rst\) 处的 nums\[j\] 即可，因为这里只用求 最小区间即可。

### Code 1\)

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int[] sums = new int[nums.length];
        for(int i = 0; i < nums.length; i ++) {
            if(i == 0) sums[i] = nums[i];
            else sums[i] = sums[i-1] + nums[i];
        }
        int rst = 0;
        for(int i = nums.length - 1; i >= 0; i--) {
            int rightBound = nums.length - 1;
            if(rst > 0) rightBound = rst + i;
            for(int j = rightBound; j >= i; j --) {
                int tmp = 0;
                if(i == 0) tmp = sums[j];
                else tmp = sums[j] - sums[i-1];
                if(tmp >= s) rst = (rst == 0) ? j - i + 1 : Math.min(rst, j - i + 1);
                else break;
            }
        }
        return rst;
    }
}
```

### 思路 2\)

1. **这里也是维护一个window，那么常常就想到 two pointers 来解决。一个指向window 左边，一个指向右边。**
2. 维护这个window，left, right pointer 都从头开始，移动right pointer直到一个window里面的sum &gt;= k, 然后移动left，看得到能 &gt;= k 的最小window。然后再移动 right pointer，如此往复。

### 复杂度

因为每一个元素最多被 left 和 right pointer 各 visit 一次，所以time complexity 是 O\(N\)

### Code

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        // 用2个pointer 维护一个 sum >= s 的窗口
        int end = 0;
        int start = 0;
        int sum = 0;
        int rst = Integer.MAX_VALUE;
        while(end < nums.length) {
            sum += nums[end];
            while(sum >= s) {
                rst = Math.min(end - start + 1, rst);
                sum -= nums[start++];
            }
            end ++;
        }
        return rst == Integer.MAX_VALUE ? 0 : rst;
    }
}
```





