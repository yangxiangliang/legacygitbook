# 471. Encode String with Shortest Length

> Given a **non-empty **string, encode the string such that its encoded length is the shortest.
>
> The encoding rule is: `k[encoded_string]`, where the encoded\_string inside the square brackets is being repeated exactly k times.
>
> **Note:**
>
> 1. _**k**_ will be a positive integer and encoded string will not be empty or have extra space.
>
> 2. You may assume that the input string contains only lowercase English letters. The string's length is at most 160.
>
> 3. If an encoding process does not make the string shorter, then do not encode it. If there are several solutions, return any of them is fine.
>
> **Example 1:**
>
> ```
> Input: "aaa"
> Output: "aaa"
> Explanation: There is no way to encode it such that it is shorter than the input string, so we do not encode it.
> ```
>
> **Example 2:**
>
> ```
> Input: "aaaaa"
> Output: "5[a]"
> Explanation: "5[a]" is shorter than "aaaaa" by 1 character.
> ```
>
> **Example 3:**
>
> ```
> Input: "aaaaaaaaaa"
> Output: "10[a]"
> Explanation: "a9[a]" or "9[a]a" are also valid solutions, both of them have the same length = 5, which is the same as "10[a]".
> ```
>
> **Example 4:**
>
> ```
> Input: "aabcaabcd"
> Output: "2[aabc]d"
> Explanation: "aabc" occurs twice, so one answer can be "2[aabc]d".
> ```
>
> **Example 5:**
>
> ```
> Input: "abbbabbbcabbbabbbc"
> Output: "2[2[abbb]c]"
> Explanation: "abbbabbbc" occurs twice, but "abbbabbbc" can also be encoded to "2[abbb]c", so one answer can be "2[2[abbb]c]".
> ```

### 思路

这道题目难度比较大，拿到题目的时候感觉会用DP方法，因为看见**Example 5，**在较长的string中会用到substring缩短后的结果。于是考虑用1个2D string array，String\[\]\[\] dp = new String\[n\]\[n\], n便是input string的长度。dp\[i\]\[j\] 表示substring\[i, j\] \(i, j inclusive\)的encode后的结果，可以从长度短的到长的，先得到较短的substring encode的结果，对于求 dp\[i\]\[j\] 的时候，其encode结果有2种情况: 

\(1\) 对于 i &lt;= k &lt; j 的 k值，遍历所有String tmp = dp\[i\]\[k\] + dp\[k+1\]\[j\] 情况，将tmp长度最短的string赋值给 dp\[i\]\[j\]，因为dp\[i\]\[k\], dp\[k+1\]\[j\]的长度肯定小于 dp\[i\]\[j\]，也就是说起encode的string的在之前已经求得，这里可以直接使用; 

**\(2\) 还有一种特别的情况**， 就是比如substring\[i, j\] = “abcabc”，其自己是repeated string，但是在\(1\)方法中，求得的结果还是"abcabc"，所以还需要check substring\[i, j\] 是不是repeated的情况。**这里check方法很巧妙，sub = substring\[i, j\] = "abcabc"，我们可以 \(sub + sub\)中找 sub第二次出现的index 是不是sub的长度，如果不是，则说明有repeated string。比如 sub + sub = abcabcabcabc，其第二次出现 sub 的index为3，小于sub.length\(\) = 6。由此可以encode sub string，repeated的次数为 \(sub.length\(\) / index\), repeated的string元素为substring\[i, i+ index-1\]，但是要注意这里不能用s.substring\[i, i + index -1\]，因为该段string可能也需要encode，所以应该用 dp\[i\]\[i+index-1\] 才行。**

### 复杂度

Time: O\(N^3\),     Space: O\(N^2\)

### Code

```java
public class Solution {
    public String encode(String s) {
        if(s.length() <= 3) return s;
        int n = s.length();
        String[][] dp = new String[n][n];
        // can not encode string whose length = 1
        for(int i = 0; i < n; i ++) dp[i][i] = s.substring(i, i+1);
        
        for(int len = 1; len <= n; len ++) {
            for(int i = 0; i + len - 1 < n; i ++) {
                int j = i + len - 1;
                for(int k = i; k < j; k ++) {
                    String tmp = dp[i][k] + dp[k+1][j];
                    if(dp[i][j] == null || tmp.length() < dp[i][j].length()) dp[i][j] = tmp;
                }
                
                // check if s.substring(i, j+1) has repeated pattern itself
                String sub = s.substring(i, j+1);
                int index = (sub + sub).indexOf(sub, 1);
                if(index < sub.length()) {
                    // repeated pattern string is dp[i][i+index-1], not using s.substring(i, i + index)
                    // 因为s.substring(i, i + index) 可能也需要encode
                    String replace = (sub.length() / index) + "[" + dp[i][i+index-1] + "]";
                    if(replace.length() < dp[i][j].length()) dp[i][j] = replace;
                }
            }
        }
        return dp[0][n-1];
    }
}
```



