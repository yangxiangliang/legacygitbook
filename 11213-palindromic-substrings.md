# 647. Palindromic Substrings

> Given a string, your task is to count how many palindromic substrings in this string.
>
> The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.
>
> **Example 1:**
>
> ```
> Input: "abc"
> Output: 3
> Explanation: Three palindromic strings: "a", "b", "c".
> ```
>
> **Example 2:**
>
> ```
> Input: "aaa"
> Output: 6
> Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
> ```
>
> **Note:**
>
> 1. The input string length won't exceed 1000.

### 思路

1. 找substring是不是palindrome，想到 substring\(i, j\) 是不是 palindrome和 substring\(i+1，j-1\) 是不是palindrome是相关的。所以想到“尾部拼接”的动态规划来计算substring\(i, j\) 是不是 palindrome。
2. 在计算过程中，每遇见一个palindrome，用个记录的counter ++ 即可。

### Code

```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        // dp[i][j] record substring [i][j] is palindromic or not
        boolean[][] dp = new boolean[n][n];
        int count = 0;
        for(int i = n - 1; i >= 0; i --) {
            for(int j = i; j < n; j ++) {
                if(i == j) dp[i][j] = true;
                else if (j == i + 1) dp[i][j] = s.charAt(i) == s.charAt(j);
                else dp[i][j] = s.charAt(i) == s.charAt(j) && dp[i+1][j-1];
                if(dp[i][j]) count ++;
            }
        }
        return count;
    }
}
```



