# 44. Wildcard Matching

> Implement wildcard pattern matching with support for `'?'`and `'*'`.
>
> ```
> '?' Matches any single character.
> '*' Matches any sequence of characters (including the empty sequence).
>
> The matching should cover the entire input string (not partial).
>
> The function prototype should be:
> bool isMatch(const char *s, const char *p)
>
> Some examples:
> isMatch("aa","a") ? false
> isMatch("aa","aa") ? true
> isMatch("aaa","aa") ? false
> isMatch("aa", "*") ? true
> isMatch("aa", "a*") ? true
> isMatch("ab", "?*") ? true
> isMatch("aab", "c*a*b") ? false
> ```

### 思路 \(2D boolean map, DP\)

思路和 [10.3.21 Regular Expression Matching](/44-hard/10321-regular-expression-matching.md)  类似，使用一个2D boolean map用 **DP** 来解决问题。dp\[i\]\[j\] 表示长度为i的s string，即 s.substring\(0, i\) 和 长度为j 的p string，即 p.substring\(0, j\) 是否match，然后该结果可以根据其substring的情况来求得。

* 注意这里dp\[i\]\[j\]中 i 和 j 表示的是长度，比如 i 所对应的string是 s.substring\(0, i\)，那么所对应的last character是s.charAt\(i-1\)，而不是s.charAt\(i\)
* 如果 s.charAt\(i-1\) == p.charAt\(j-1\) 或则 p.charAt\(j-1\) == '?'，那么表示s.charAt\(i-1\) 和 p.charAt\(j-1\) 需要匹配，则                    dp\[i\]\[j\] = dp\[i-1\]\[j-1\]
* 如果 p.charAt\(j-1\) == '\*'，那么表示p.charAt\(j-1\)可以匹配0个或则多个字符。匹配0个字符时，相当于dp\[i\]\[j-1\]的情况；匹配多个字符时，相当于dp\[i-1\]\[j\]的情况，因为如果dp\[i\]\[j\] 为true而且可以匹配多个字符，那么dp\[i-1\]\[j\]也一定为true。所以此时有：   dp\[i\]\[j\] = dp\[i-1\]\[j\] \|\| dp\[i\]\[j-1\]

### 复杂度

O\(n \* m\), n 和 m 分别为 2 个input string的长度

### Code

```java
public class Solution {
    public boolean isMatch(String s, String p) {
        int n = s.length();
        int m = p.length();
        boolean[][] dp = new boolean[n+1][m+1];
        dp[0][0] = true;
        // 处理s == "" 的情况
        for(int i = 1; i <= m; i ++) {
            // 此时只有 p 中全为"***"的时候，才可能match
            if(p.charAt(i-1) == '*') dp[0][i] = true;
            else break;
        }
        // 处理 p == "" 的情况
        for(int i = 1; i <= n; i ++) dp[i][0] = false;
        for(int i = 1; i <= n; i ++) {
            for(int j = 1; j <= m; j ++) {
                if(s.charAt(i-1) == p.charAt(j-1) || p.charAt(j-1) == '?') dp[i][j] = dp[i-1][j-1];
                else if (p.charAt(j-1) == '*') dp[i][j] = dp[i-1][j] || dp[i][j-1];
                else dp[i][j] = false;
            }
        }
        return dp[n][m];
    }
}
```



