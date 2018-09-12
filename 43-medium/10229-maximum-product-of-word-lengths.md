# 318. Maximum Product of Word Lengths

> Given a string array`words`, find the maximum value of`length(word[i]) * length(word[j])`where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.
>
> **Example 1:**
>
> Given`["abcw", "baz", "foo", "bar", "xtfn", "abcdef"]`  
> Return`16`  
> The two words can be`"abcw", "xtfn"`.
>
> **Example 2:**
>
> Given`["a", "ab", "abc", "d", "cd", "bcd", "abcd"]`  
> Return`4`  
> The two words can be`"ab", "cd"`.
>
> **Example 3:**
>
> Given`["a", "aa", "aaa", "aaaa"]`  
> Return`0`  
> No such pair of words.

### 思路

1. 一开始只想到brute force的方法，见下面 **code 1\)**
2. 这里关键就是更有效率的找 **2个string是否有相同的character。**由于都是lower case，只有26个情况，想到可以用int\(32bit\) value 来表示1个string，value \|= 1 &lt;&lt; \(string.charAt\(i\) - 'a'\)，这样value的前26bit每一位表示是否有对应的character，如果value1 & value2 == 0，则说明两个string没有共同的character。**与这里思路类似的有**[**Single Number II**](https://leetcode.com/problems/single-number-ii/description/)

### Code 1\) Brute Force

```java
public class Solution {
    public int maxProduct(String[] words) {
        int max = 0;
        for(int i = 0; i < words.length - 1; i ++) {
            HashSet<Character> set = new HashSet();
            for(char c : words[i].toCharArray()) set.add(c);
            for(int j = i + 1; j < words.length; j ++) {
                boolean valid = true;
                for(char c : words[j].toCharArray()) {
                    if(set.contains(c)) {
                        valid = false;
                        break;
                    }
                }
                if(valid) max = Math.max(words[i].length() * words[j].length(), max);
            }
        }
        return max;
    }
}
```

### Code 2\) Bit Manipulation

```java
public class Solution {
    public int maxProduct(String[] words) {
        int max = 0;
        int[] value = new int[words.length];
        
        for(int i = 0; i < words.length; i ++) {
            String tmp = words[i];
            for(char c : tmp.toCharArray()) value[i] |= 1 << (c - 'a');
        }
        for(int i = 0; i < words.length - 1; i ++) {
            for(int j = i + 1; j < words.length; j ++) {
                if((value[i] & value[j]) == 0) max = Math.max(max, words[i].length() * words[j].length());
            }
        }
        return max;
    }
}
```



