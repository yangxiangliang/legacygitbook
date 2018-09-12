# 524. Longest Word in Dictionary through Deleting

> Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.
>
> **Example 1:**
>
> ```
> Input:
> s = "abpcplea", d = ["ale","apple","monkey","plea"]
>
> Output: 
> "apple"
> ```
>
> **Example 2:**
>
> ```
> Input:
> s = "abpcplea", d = ["a","b","c"]
>
> Output: 
> "a"
> ```

### 思路

1. 关键是怎么比较s 和 d 中的word，看word是否可以通过s delete一些character来得到。用一个pointer scan s中，看是否能找到word 中的每一个character，如果scan 结束了都不能找到word中的所有character。则说明不能delete s 中的character来得到该word string。

### Code

```java
class Solution {
    public String findLongestWord(String s, List<String> d) {
        String rst = "";
        for(String word : d) {
            if(valid(s, word)) {
                if(word.length() > rst.length() || word.length() == rst.length() && word.compareTo(rst) < 0) {
                    rst = word;
                }
            }
        }
        return rst;
    }

    private boolean valid(String s, String word) {
        int start = 0;
        for(int i = 0; i < word.length(); i ++) {
            while(start < s.length() && s.charAt(start) != word.charAt(i)) start ++;
            if(start == s.length()) return false;
            start ++;
        }
        return true;
    }
}
```



