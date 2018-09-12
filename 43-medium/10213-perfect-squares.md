# 279. Perfect Squares

> Given a positive integern, find the least number of perfect square numbers \(for example,`1, 4, 9, 16, ...`\) which sum to n.
>
> For example, given n=`12`, return`3`because`12 = 4 + 4 + 4`; given n=`13`, return`2`because`13 = 4 + 9`.

### 思路

找sum 等于n的perfect square的个数，等于 找 perfect squares 的和等于 \(n - j \* j\)的个数 + 1。由于这里会有很多重复计算，想到用dynamic programming，用array int\[\] count = new int\[n+1\]，其中count\[i\]记录sum等于i的perfect squares的个数。

### Code

```java
public class Solution {
    public int numSquares(int n) {
        int[] count = new int[n+1];
        for(int i = 1; i <= n; i ++) {
            // 遍历 j = 1 到 j*j < i的所有j，找到perfect squares个数最少的组合
            for(int j = 1; j * j <= i; j ++) {
                count[i] = count[i] == 0 ? count[i - j*j] + 1 : Math.min(count[i], count[i - j*j] + 1);
            }
        }
        return count[n];
    }
}
```



