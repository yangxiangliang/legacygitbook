# 494. Target Sum

> You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+`and `-`. For each integer, you should choose one from`+`and`-`as its new symbol.
>
> Find out how many ways to assign symbols to make sum of integers equal to target S.
>
> **Example 1:**
>
> ```
> Input: nums is [1, 1, 1, 1, 1], S is 3. 
> Output: 5
> Explanation: 
>
> -1+1+1+1+1 = 3
> +1-1+1+1+1 = 3
> +1+1-1+1+1 = 3
> +1+1+1-1+1 = 3
> +1+1+1+1-1 = 3
>
> There are 5 ways to assign symbols to make the sum of nums be target 3.
> ```
>
> **Note:**
>
> 1. The length of the given array is positive and will not exceed 20.
> 2. The sum of elements in the given array will not exceed 1000.
> 3. Your output answer is guaranteed to be fitted in a 32-bit integer.

### 思路 1）

思路类似于 [11.3.4 Expression Add Operators](/1134-expression-add-operators.md)，但是比其容易很多，只有+，- 号。还是用backtracking来做，但是不同的是不是返回path，而是返回最终结果等于S的个数。那么recursion函数每次返回能够成对应target的个数即可。

### 复杂度

Time: O\(2^N \* N\) ，因为每个数字都有+，-， 2种情况，遍历其所有条件。一共有O\(2^N\)情况，相当于解空间中2^N个Node，每个node计算又需要N。

### Code 1\)

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        return find(nums, 0, 0, S);
    }

    private int find(int[] nums, int start, int value, int S) {
        if(start == nums.length) {
            if(value == S) return 1;
            return 0;
        }
        int rst = 0;
        // add nums[start]
        rst += find(nums, start + 1, value + nums[start], S);
        // minus nums[start]
        rst += find(nums, start + 1, value - nums[start], S);
        return rst;
    }
}
```

### 思路 2）

1. 想想其中肯定有不少重复计算，能不能避免一些重复计算。可以想到DP，或则正面“演绎法”。因为nums\[0\] 到 nums\[i\] 能得到 value 是和nums\[0\] 到nums\[i-1\] 所有元素能得到的值有关系的，即前者等于后者 “+” 或则 “-” nums\[i\]。那么就想可不可以用从nums\[0\]开始，一步一步往右移动，得到sub array能得到的所有值的情况，以及有多少种方法得到每个值。
2. 想想怎么储存这些信息，每个nums\[0\] 到 nums\[i\] 所能得到的值可以用HashMap来存，作为key，那么value就作为得到该值的不同符号安排的方法数量。然后用List&lt;&gt; 把这些hashMap串起来，list.get\(i\)的hashMap就对应 nums\[0\]到nums\[i\]的信息。

### Code 2\)

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        List<HashMap<Integer, Integer>> memo = new ArrayList();
        for(int i = 0; i < nums.length; i ++) {
            HashMap<Integer, Integer> cur = new HashMap();
            if(i == 0) {
                cur.put(-nums[i], 1);
                // 可能 nums[i] = 0，即nums[i] 存在于map中，所以加上其已经得知的构成的符号安排方法数量
                cur.put(nums[i], 1 + cur.getOrDefault(nums[i], 0));
            } else {
                HashMap<Integer, Integer> pre = memo.get(i-1);
                for(int preCount : pre.keySet()) {
                    int tmp1 = preCount - nums[i];
                    int tmp2 = preCount + nums[i];
                    cur.put(tmp1, pre.get(preCount) + cur.getOrDefault(tmp1, 0));
                    cur.put(tmp2, pre.get(preCount) + cur.getOrDefault(tmp2, 0));
                }
            }
            memo.add(cur);
        }
        return memo.get(memo.size() - 1).getOrDefault(S, 0);
    }
}
```





