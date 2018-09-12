# 689. Maximum Sum of 3 Non-Overlapping Subarrays

> In a given array `nums`of positive integers, find three non-overlapping subarrays with maximum sum.
>
> Each subarray will be of size `k`, and we want to maximize the sum of all `3*k`entries.
>
> Return the result as a list of indices representing the starting position of each interval \(0-indexed\). If there are multiple answers, return the lexicographically smallest one.
>
> **Example:**
>
> ```
> Input: [1,2,1,2,6,7,5,1], 2
> Output: [0, 3, 5]
> Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
> We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
> ```
>
> **Note:**
>
> * `nums.length`will be between 1 and 20000.
>
> * `nums[i]`will be between 1 and 65535.
>
> * `k`will be between 1 and floor\(nums.length / 3\).

### 思路

1. 因为这里是取3个连续为k size的subarray相加，所以应该把nums\[i, i+k-1\] 这样的subarray看做一个整体，然后研究3个这样的一段一段的subarray的和为最大。可以首先遍历一遍用int\[\] count来存每一段为k size的subarray的sum值。比如count\[i\] 表示的是nums\[i\] 到nums\[i+k-1\] 之间所有元素的和。
2. 首先想3个subarray感觉有点复杂，先想2个subarray的和求最大值。可以想到到每一个点count\[i\]，取不取count\[i\]这一段的值，可以分2种情况讨论：
   1. 如果不取count\[i\]值，那么在start index 在\[0, i\]中的2个subarray的最大和等于start index在\[0, i-1\]中的2个subarray的最大和。
   2. 如果取count\[i\]值，因为不能overlap，则该结果变成count\[i\] + \(start index最多到\[i-k\]处的最大值\)。
3. 这里感觉和DP有关系了，因为\[0, i\]中取2段 subarray的情况和\[0, i-1\]中取2段subarray的情况有关系，因为还要看start index 在   \[i-k\] 之前的最大subarray。所以我们可以先用1个DP记录到某个index i处的1个subarray的最大值。然后根据这个再来计算到某个index i处的2个subarray的最大值。然后再来计算到某个index i 处的3个subarray的最大值即可。
4. **注意:** 因为最后如果有多个max value，index需要lexicographically 较小的，即计算过程中，尽量找index较小者即可。

### Code

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int[] count = new int[nums.length - k + 1]; // count[i] = sum of nums[i] to nums[i+k-1]
        for(int i = 0; i < count.length; i++) {
            if(i == 0) {
                for(int j = 0; j < k; j ++) count[i] += nums[j];
            } else count[i] = count[i-1] - nums[i-1] + nums[i+k-1];
        }
        
        // dp for 1 subarray, value is max value of 1 subarray whose start index <= i
        int[] dp1 = new int[count.length];
        // key is index i in (int[] dp1), value is start index of max value of 1 subarray whose start index <= i
        HashMap<Integer, Integer> map1 = new HashMap();
        // dp2 for 2 subarrays sum, dp2[i] value is max value of 2 subarray whose start index <= i
        int[] dp2 = new int[count.length];
        // key is index i in dp2, value is related start index number for max values' 2 subarrays
        HashMap<Integer, List<Integer>> map2 = new HashMap();
        // dp3 for 3 subarrays sum, dp3[i] value is max value of 3 subarray whose start index <= i
        int[] dp3 = new int[count.length];
        // key is index i in dp3, value is related start index number for max values' 3 subarrays
        HashMap<Integer, List<Integer>> map3 = new HashMap();
        for(int i = 0; i < count.length; i ++) {
            if(i == 0) {
                dp1[i] = count[0];
                map1.put(i, 0);
            } else {
                if(count[i] > dp1[i-1]) map1.put(i, i);
                else map1.put(i, map1.get(i-1));
                dp1[i] = Math.max(dp1[i-1], count[i]);
            }
            
            // i >= k时，才能放下2个subarrays
            if(i >= k) {
                // record 2 start index of 2 subarrays which gives max value for start index <= i
                List<Integer> tmp = new ArrayList();
                if(i == k) {
                    dp2[i] = count[i] + count[0];
                    tmp.add(0);
                    tmp.add(i);
                } else {
                    // 如果取count[i]，那么找 (index<=i-k) 中最大的1个subarray即可, 从dp1中找
                    if(count[i] + dp1[i-k] > dp2[i-1]) {
                        dp2[i] = count[i] + dp1[i-k];
                        tmp.add(map1.get(i-k));
                        tmp.add(i);
                    } else {
                        dp2[i] = dp2[i-1];
                        tmp = map2.get(i-1);
                    }
                }
                map2.put(i, tmp);
            }
            
            // i >= 2*k时，才能放下3个subarrays
            if(i >= 2*k) {
                List<Integer> tmp = new ArrayList();
                if(i == k) {
                    dp3[i] = count[i] + count[i-k] + count[i-2*k];
                    tmp.add(i-2*k);
                    tmp.add(i-k);
                    tmp.add(i);
                } else {
                    // 如果count[i]取值，那么找(index <= i-k) 中最大的2个subarray和即可，从dp2中找
                    if(count[i] + dp2[i-k] > dp3[i-1]) {
                        dp3[i] = count[i] + dp2[i-k];
                        tmp.addAll(map2.get(i-k));
                        tmp.add(i);
                    } else {
                        dp3[i] = dp3[i-1];
                        tmp = map3.get(i-1);
                    }
                }
                map3.put(i, tmp);
            }
        }
        
        List<Integer> list = map3.get(count.length - 1);
        int[] rst = new int[list.size()];
        for(int i = 0; i < list.size(); i ++) rst[i] = list.get(i);
        return rst;
    }
}
```



