# 525. Contiguous Array

> Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.
>
> **Example 1:**
>
> ```
> Input: [0,1]
> Output: 2
> Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.
> ```
>
> **Example 2:**
>
> ```
> Input: [0,1,0]
> Output: 2
> Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
> ```
>
> **Note:**
>
> The length of the given binary array will not exceed 50,000.

### 思路

1. 这个题目很容易想到O\(N^2\) 的方法，就是用int\[\] count 记录每一段的0, 1的数量。比如 count\[i\] 记录的就是nums\[0\]到nums\[i\]的1比0多的数量，比如遇见1就++，遇见0 就 --。counter等于0的时候就表示 0 和 1的数量相等。其实就是类似 “左右括号”的问题，只不过这里换成0，1了。然后任意一段continuous array nums\[i, j\]的0，1 相差的数量 就是count\[j\] - count\[i-1\] 得到即可。但是这个方法太慢，显然不行。
2. **重点来了！这个方法用了好多地方，其实就是 two sum的基本方法。因为这里要找的就是count\[j\] - count\[i-1\] == 0 的情况。那么在scan nums 数组的时候，遇见当前count\[j\]的值的时候，只需要知道前面是否有count\[i\]的值等于count\[j\] 即可。等于的话，他们相减就是0，他们之间的subarray就是符合条件的。显然为了fast lookup，需要hashMap 来存储已知的count\[i\] 的值 和其对应的index。（类似题目：**[**LC325**](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/description/)**）**

### Code

```java
class Solution {
    public int findMaxLength(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int count = 0; // ++ when meets 1, -- when meets 0
        int rst = 0;
        for(int i = 0; i < nums.length; i ++) {
            if(nums[i] == 1) count++;
            if(nums[i] == 0) count--;
            if(count == 0) rst = Math.max(rst, i + 1);
            else if(map.containsKey(count)) rst = Math.max(rst, i - map.get(count));

            // 注意如果map已经有了当前count值，则不要update map
            // 因为对于某一个count值，只需要store index i 值最小的情况即可，因为这里求max size subarray
            // index i越小，之后越可能得到max size的结果
            if(!map.containsKey(count)) map.put(count, i);
        }
        return rst;
    }
}
```



