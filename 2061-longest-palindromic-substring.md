# 5. Longest Palindromic Substring

> Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s **is 1000.
>
> **Example 1:**
>
> ```
> Input: "babad"
> Output: "bab"
> Note: "aba" is also a valid answer.
> ```
>
> **Example 2:**
>
> ```
> Input: "cbbd"
> Output: "bb"
> ```

### 思路

1. 首先s.substring\(i, j\) 是palindromic的前提是 s.substring\(i+1, j-1\) 也是 palindromic 而且 i 和 j 处的character相同。所以 substring\(i, j\) 是否是palindromic是由substring\(i+1, j-1\)是否是 palindromic 传递过来的。由此可以想到 DP 来操作。

### Code

```java
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        boolean[][] map = new boolean[len][len];
        String rst = "";

        for(int i = len - 1; i >= 0; i--) {
            for(int j = i; j < len; j ++) {
                if(j == i) map[i][j] = true;
                else if (j == i + 1) map[i][j] = s.charAt(i) == s.charAt(j);
                else map[i][j] = s.charAt(i) == s.charAt(j) && map[i+1][j-1];

                if(map[i][j] && (j - i + 1) > rst.length()) rst = s.substring(i, j + 1);
            }
        }
        return rst;
    }
}
```



