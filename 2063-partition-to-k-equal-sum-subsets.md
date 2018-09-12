# 698. Partition to K Equal Sum Subsets

> Given an array of integers `nums`and a positive integer`k`, find whether it's possible to divide this array into `k`non-empty subsets whose sums are all equal.
>
> **Example 1:**
>
> ```
> Input: nums = [4, 3, 2, 3, 5, 2, 1], k = 4
> Output: True
> Explanation: It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.
> ```
>
> **Note:**
>
> * 1 &lt;= k &lt;= len\(nums\) &lt;= 16
> * 0 &lt; nums\[i\] &lt; 10000

### 思路

1. 首先求所有元素的和，看其能不能被K整除。只有能被 K 整除的情况，才能被分成相等的K个subset。
2. 因为 array 也不是sorted的，之间的元素相加也没有规律，只有用DFS一个 一个尝试，如果尝试到subset 等于 \(sum / k\)，那么看剩下的元素能不能分成 \(k - 1\) 个相等的subset，如此 "backtracking" 一个一个尝试下去。

### Code

```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        int sum = 0;
        for(int num : nums) sum += num;
        if(sum % k != 0) return false;

        boolean[] visited = new boolean[nums.length];
        return canPartition(nums, visited, 0, k, 0, sum / k);
    }

    private boolean canPartition(int[] nums, boolean[] visited, int start, int k, int preSum, int target) {
        if(k == 1) return true;
        if(preSum == target) return canPartition(nums, visited, 0, k-1, 0, target);
        for(int i = start; i < nums.length; i ++) {
            if(!visited[i]) {
                visited[i] = true;
                if(canPartition(nums, visited, i+1, k, preSum+nums[i], target)) return true;
                visited[i] = false;
            }
        }
        return false;
    }
}
```



