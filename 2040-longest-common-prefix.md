# 14. Longest Common Prefix

> Write a function to find the longest common prefix string amongst an array of strings.
>
> If there is no common prefix, return an empty string`""`.
>
> **Example 1:**
>
> ```
> Input: ["flower","flow","flight"]
> Output: "fl"
> ```
>
> **Example 2:**
>
> ```
> Input: ["dog","racecar","car"]
> Output: ""
> Explanation: There is no common prefix among the input strings.
> ```
>
> **Note:**
>
> All given inputs are in lowercase letters `a-z`.

### 思路

因为这里需要找到所有string的common prefix。所以可以以任何一个string为标准target string，把target string和其他所有string一个character一个character相比较，看其是否有可能是common prefix即可。

### Code

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length == 0) return "";

        // 把target string作为标准
        String target = strs[0];
        int i = 0;
        for(; i < target.length(); i ++) {
            int j = 1;

            // 看target.charAt(i) 是否也出现在其他str的位置i处，如果是，则其为common prefix中的char
            // 反之，其不可能是common prefix中的char
            // 注意：i 可能超出 strs[j]的长度范围，则此时target.charAt(i)也不可能是common prefix的一部分
            for(; j < strs.length; j ++) {
                if(i < strs[j].length() && target.charAt(i) == strs[j].charAt(i)) continue;
                break;
            }
            if(j < strs.length) break;
        }
        return target.substring(0, i);
    }
}
```



