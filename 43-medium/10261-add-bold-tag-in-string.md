# 616. Add Bold Tag in String

> Given a string **s **and a list of strings **dict**, you need to add a closed pair of bold tag`<b>`and`</b>`to wrap the substrings in s that exist in dict. If two such substrings overlap, you need to wrap them together by only one pair of closed bold tag. Also, if two substrings wrapped by bold tags are consecutive, you need to combine them.
>
> **Example 1:**
>
> ```
> Input: 
> s = "abcxyz123"
> dict = ["abc","123"]
> Output:
> "<b>abc</b>xyz<b>123</b>"
> ```
>
> **Example 2:**
>
> ```
> Input: 
> s = "aaabbcc"
> dict = ["aaa","aab","bc"]
> Output:
> "<b>aaabbc</b>c"
> ```

### 思路

1. 这里因为overlap或则连续的word，都需要放在同一个&lt;b&gt; 里面。一开始想在scan input string s的时候，来查哪些Word需要bold，但是因为overlap的情况，感觉不好入手。
2. 那么这里想可不可以分2步
   1. 先用一个boolean\[\] array 表示每一位character是否需要bold。
   2. 根据上面得到的boolean\[\] array 把需要bold的continuous那一段前后加上&lt;b&gt; 符号即可。

### Code

```java
class Solution {
    public String addBoldTag(String s, String[] dict) {
        boolean[] bold = new boolean[s.length()];

        int end = -1;
        for(int i = 0; i < s.length(); ++i) {
            for(String word : dict) {
                if(s.substring(i).startsWith(word)) {
                    end = Math.max(end, i + word.length() - 1);
                }
            }
            bold[i] = end >= i;
        }

        StringBuilder rst = new StringBuilder();
        for(int i = 0; i < s.length(); ++i) {
            if(!bold[i]) {
                rst.append(s.charAt(i));
                continue;
            }
            int j = i; 
            while(j < s.length() && bold[j]) j ++;
            rst.append("<b>" + s.substring(i, j) + "</b>");
            i = j - 1;
        }
        return rst.toString();
    }
}
```



