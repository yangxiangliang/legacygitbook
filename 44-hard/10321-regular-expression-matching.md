# 10. Regular Expression Matching

> Implement regular expression matching with support for`'.'`and`'*'`.
>
> ```
> '.' Matches any single character.
> '*' Matches zero or more of the preceding element.
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
> isMatch("aa", "a*") ? true
> isMatch("aa", ".*") ? true
> isMatch("ab", ".*") ? true
> isMatch("aab", "c*a*b") ? true
> ```

### 思路 1\)

使用recursion容易想到和理解，但是不够efficient。这里遇见p中"."和character的情况比较容易处理，只用看和s中对应的是否match即可，难点在于"\*"的情况。

* 因为"\*"可能match之前的0个或则多个character。如果p.charAt\(1\) == '\*'的情况下，先检查s.charAt\(0\), p.charAt\(0\)是否match，如果不match，那么说明p.charAt\(1\)的"\*"只有可能match 0个前面的character，所以看s 和 p.substring\(2\)是否match即可。
* 当然在p.charAt\(1\) == ‘\*’的情况下，如果s.charAt\(0\)和p.charAt\(0\)match，这里分2种情况：1\)可能还是可以认为p.charAt\(1\)的"\*"可能被看作match 0个前面的character，如果s和p.substring\(2\) match，那么也可以认为s和p match; 2\)可能p.charAt\(1\)的"\*"是match 1个或则多个前面的character，因为s.charAt\(0\)已经被match掉了，那么接着看 s.substring\(1\)和p是否match 即可。

### Code 1\)

```java
public class Solution {
    public boolean isMatch(String s, String p) {
        if(p.length() == 0) return s.length() == 0;
        if(p.length() == 1) return s.length() == 1 && (p.charAt(0) == '.' || p.charAt(0) == s.charAt(0));
        // p.charAt(0)和s.charAt(0)必须match，然后recursion看各自的 substring 是否match即可
        if(p.charAt(1) != '*') {
            if(s.length() < 1) return false;
            return (p.charAt(0) == '.' || p.charAt(0) == s.charAt(0)) && isMatch(s.substring(1), p.substring(1));
        }
        // 此时p.charAt(1) == '*'
        // 1) 如果s.charAt(0)和p.charAt(0)不match，那么看s和p.substring(2)是否match
        // 2) 如果s.charAt(0)和p.chatAt(0) match，那么s和p.substring(2)match也可以，s.substring(1)和p相互match也可以
        while(s.length() > 0 && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.')) {
            if(isMatch(s, p.substring(2))) return true;
            s = s.substring(1);
        }
        return isMatch(s, p.substring(2));
    }
}
```

### 思路 2\)

由上面的思路可以看出，两个string是否match和其substring是有关系的，由此可以想到可以利用 **DP** 来解决。首先构建boolean\[\]\[\] dp = new int\[s.length\(\)+1\]\[p.length\(\)+1\] 数组，dp\[i\]\[j\]表示长度为i的数组s\(即s.substring\(i\)\)和长度为j的数组p\(即p.substring\(j\)\)是否match，boolean值的传递分下面几种情况，计算dp\[i\]\[j\]的值:

* 如果p\[j-1\] != '\*', 那么需要s\[i-1\]和p\[j-1\]match。需要 s\[i-1\] == p\[j-1\] or p\[j-1\] == '.'，那么dp\[i\]\[j\] = dp\[i-1\]\[j-1\]
* 难点在p\[j-1\] = '\*'的时候
  * 如果dp\[i\]\[j-2\] = true，则dp\[i\]\[j\]也可以为true，相当于p\[j-1\] = '\*'和p\[j-2\]的character match的是empty
  * 如果p\[j-1\] = '\*'和p\[j-2\]的character可以match 1个或则多个，那么需要p\[j-2\]和s\[i-1\]先match，然后看dp\[i-1\]\[j\]是否match。因为p\[j-1\] = '\*'可能match到了s\[i-1\]之前的character，也可能没有match到s\[i-1\]之前。 **可以想如果p\[j-2\]和s\[i-1\]match一个或则多个的话，要想dp\[i\]\[j\]是true，那么dp\[i-1\]\[j\]一定也是true。因为如果p\[j-2\]和s\[i-1\] match一个，那么dp\[i-1\]\[j\] 可以看成    p\[j-1\] = '\*' 表示0个preceding character，那么dp\[i-1\]\[j\]应该是true。如果p\[j-2\]和s\[i-1\] match n个，那么dp\[i-1\]\[j\]可以看成   p\[j-1\] = '\*' match preceding \(n-1\) 个character，所以dp\[i-1\]\[j\] 也肯定是true.**

### Code 2\)

```java
public class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        boolean[][] dp = new boolean[m+1][n+1];
        dp[0][0] = true;
        //初始化第0行,除了[0][0]全为false，毋庸置疑，因为空串p只能匹配空串，其他都无能匹配
        for(int i = 1; i <= m; i ++) dp[i][0] = false;
        //初始化第0列，只有X*能匹配空串，如果有*，它的真值一定和p[0][j-2]的相同（略过它之前的符号）
        for(int j = 1; j <= n; j ++) dp[0][j] = j > 1 && p.charAt(j-1) == '*' && dp[0][j-2];

        for(int i = 1; i <= m; i ++) {
            for(int j = 1; j <= n; j ++) {
                if(p.charAt(j-1) == '*') {
                    // 这里j-1才是正常字符串中的字符位置
                    // 要不*当空，要不就只有当前字符匹配了*之前的字符，才有资格传递dp[i-1][j]真值
                    dp[i][j] = dp[i][j-2] || (p.charAt(j-2) == '.' || p.charAt(j-2) == s.charAt(i-1)) && dp[i-1][j];
                } else {
                    // 只有当前字符完全匹配，才有资格传递dp[i-1][j-1] 真值
                    dp[i][j] = (p.charAt(j-1) == '.' || p.charAt(j-1) == s.charAt(i-1)) && dp[i-1][j-1];
                }
            }
        }
        return dp[m][n];
    }
}
```



