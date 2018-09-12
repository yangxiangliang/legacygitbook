# 639. Decode Ways II

> A message containing letters from`A-Z`is being encoded to numbers using the following mapping way:
>
> ```
> 'A' -> 1
> 'B' -> 2
> ...
> 'Z' -> 26
> ```
>
> Beyond that, now the encoded string can also contain the character '\*', which can be treated as one of the numbers from 1 to 9.
>
> Given the encoded message containing digits and the character '\*', return the total number of ways to decode it.
>
> Also, since the answer may be very large, you should return the output mod 10^9 + 7.
>
> **Example 1:**
>
> ```
> Input: "*"
> Output: 9
> Explanation: The encoded message can be decoded to the string: "A", "B", "C", "D", "E", "F", "G", "H", "I".
> ```
>
> **Example 2:**
>
> ```
> Input: "1*"
> Output: 9 + 9 = 18
> ```
>
> **Note:**
>
> 1. The length of the input string will fit in range \[1, 10^5\].
> 2. The input string will only contain the character '\*' and digits '0' - '9'.

### 思路

1. 这个题目和 [20.15 Decode Ways](/2015-decode-ways.md) 类似，区别就是加了“\*” 可以作为 ‘1’ - ‘9’ 的special character。因为分析可得，前i的character decode的方法数量和 前\(i-1\), 前\(i-2\) 可以decode的方法数量有关系，可以画解空间树，可以采用DP，用dp\[i\] 来记录前面 i 个character 能 decode的数量。
2. 基本思路如上，更复杂的地方就是遇见‘\*’的character的时候，需要仔细的分情况分析，因为‘\*’ 可能是好几种valid的character。

### Code

```java
class Solution {
    public int numDecodings(String s) {
        // 因为题目中说结果可能很大，超出int范围，所以这里用 long 储存中间结果
        long[] dp = new long[s.length()];
        for(int i = 0; i < s.length(); i ++) {
            char c = s.charAt(i);
            if(i == 0) {
                if(c == '*') dp[i] = 9;
                else if(c == '0') return 0;
                else dp[i] = 1;
            } else if (c == '*') {
                // regard s.charAt(i) represents a single number
                //因为‘*’有9种可能性，所以是 9*dp[i-1], 而不是 9+dp[i-1]
                dp[i] += dp[i-1] * 9;
                // regard s.charAt(i-1) and s.charAt(i) represents double digit number
                int possibility = 0; // 表示char(i-1), char(i) 能构成的double digit number的可能性
                if(s.charAt(i-1) == '1') possibility = 9;
                if (s.charAt(i-1) == '2') possibility = 6;
                if(s.charAt(i-1) == '*') possibility = 15;
                dp[i] += i > 1 ? dp[i-2] * possibility : possibility;
            } else {
                // c == 某个数字，如果 c == '0'，表示char(i) 不可能单独成为一位decode成single digit number
                if(c != '0') dp[i] += dp[i-1];
                int possibility = 0;
                if(s.charAt(i-1) == '*') possibility = 2;
                if(s.charAt(i-1) == '1' || s.charAt(i-1) == '2') possibility = 1;
                // c > '6'的时候，char(i-1), char(i) 只有可能decode成 “1c”，才能不超出26的范围
                if(c > '6' && possibility > 0) possibility = s.charAt(i-1) == '2' ? 0 : 1;
                dp[i] += i > 1 ? possibility * dp[i-2] : possibility;
            }
            dp[i] %= 1000000007; // 根据题目要求，结果取mod (10^9 + 7) 即可
        }
        return (int) dp[dp.length - 1];
    }
}
```



