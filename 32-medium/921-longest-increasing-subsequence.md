# 300. Longest Increasing Subsequence

> Given an unsorted array of integers, find the length of longest increasing subsequence.
>
> For example,  
> Given`[10, 9, 2, 5, 3, 7, 101, 18]`,  
> The longest increasing subsequence is`[2, 3, 7, 101]`, therefore the length is`4`. Note that there may be more than one LIS combination, it is only necessary for you to return the length.
>
> Your algorithm should run in O\(n^2\) complexity.
>
> **Follow up:** Could you improve it to O\(n\* log\(n\)\) time complexity?

### 思路

1. 一开始的时候想DP，一个一个往后面找longest increasing subsequence。想到的就是用 int\[\] dp 记录，dp\[i\] 表示从nums最开始到 nums\[i\] 位置时longest increasing subsequence的长度。**但是发现这样选择dp后很难找出递推关系，因为没法记录dp\[i\] 时longest increasing subsequence 中的最大值，这样在判断后面的 dp\[j\] \(j &gt; i\) 的值的时候，没法判断其和dp\[i\]的关系了。**

2. 所以选择DP，int\[\] dp 表示的的意思的时候需要改变。**因为每个longest increasing subsequence肯定是以一个nums\[i\] 为结尾的，那么dp\[i\] 可以表示 ”以 i 结尾的longest increasing subsequence的长度“**。这样dp\[i+1\]的长度时候，因为需要\[i+1\] 为结尾，那么就是nums\[i+1\] 和之前 nums\[0\],....nums\[i\] 的值比较，如果nums\[i+1\] 更大，而且dp\[i+1\] &lt; dp\[j\] + 1 \(j &lt;= i\)，那么可以更新dp\[i+1\] 值，一直遍历下去即可。**\(其实就是DP中的”选点拼接“\)**

### 复杂度

Time: O\(N^2\)

### Code

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int rst = 0, n = nums.length;
        int[] dp = new int[n];
        for(int i = 0; i < n; i ++) {
            dp[i] = 1; // 至少长度是1，即单独一个元素是sequence
            for(int j = 0; j < i; j ++) {
                if(nums[i] > nums[j]) {
                    if(dp[j] + 1 > dp[i]) dp[i] = dp[j] + 1;
                }
            }
            rst = Math.max(rst, dp[i]);
        }
        return rst;
    }
}
```

### Follow up

这里需要时间复杂度是 O\(N \* log\(N\)\)。思路确实不好想，我也是看的别人的解答，[请见这里](https://www.felix021.com/blog/read.php?1587)。

1. 简单来说，就是用1 个序列int\[\] rst 记录每个长度的increasing subarray的结尾的最小元素。因为记录该长度的increasing subarray的结尾最小元素时候，往后遍历的时候才能尽可能的往后面添加新元素，得到更长的increasing subarray。比如 rst\[i\] 表示的就是长度为\(i+1\)的increasing subarray中末尾最小的元素值。
2. 容易得证，rst 数组自己是一个递增序列。所以可以用binary search 来找新元素在其中的位置。如果新元素比目前 rst 的最大\(最右\)元素还大，那说明应该增加increasing subarray的长度了。

### Code 2\)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        if(n == 0) return 0;
        int[] rst = new int[n];
        int size = 1; // 初始最开始的元素nums[0] 作为长度为1的sub array
        rst[0] = nums[0];
        for(int i = 1; i < n; i ++) {
            int pos = binarySearch(rst, 0, size - 1, nums[i]);
            rst[pos] = nums[i];
            if(pos == size) size ++;
        }
        return size;
    }

    // 该函数相当于就是返回target应该插入到有序数列rst[] 中的时候，应该插入的index 值
    private int binarySearch(int[] nums, int start, int end, int target) {
        if(target > nums[end]) return end + 1;
        while(start < end) {
            int mid = (start + end) / 2;
            if(nums[mid] == target) return mid;
            if(nums[mid] < target) start =  mid + 1;
            // (注意) 这里用 end = mid，而不是 end = mid - 1
            // 比如(1, 3...) 中间插入2，需要插入的index = 1，比如mid = 1，target(2) 比 3小
            // 但是2有可能就是插入3的位置，所以end = 1，而不是 (mid - 1)
            else end = mid;
        }
        return start;
    }
}
```



