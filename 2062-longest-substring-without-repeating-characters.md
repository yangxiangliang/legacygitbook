# 3. Longest Substring Without Repeating Characters

> Given a string, find the length of the **longest substring **without repeating characters.
>
> **Examples:**
>
> Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.
>
> Given`"bbbbb"`, the answer is`"b"`, with the length of 1.
>
> Given`"pwwkew"`, the answer is`"wke"`, with the length of 3. Note that the answer must be a **substring**,`"pwke"`is a subsequence and not a substring.

### 思路

1. 典型的 “two pointers” 题目，用 two pointers 来维护一个没有重复 characters的window。记录该window的最大长度，也就是所求的longest substring。

### Code

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // start, end 是维护的window的起始和末尾2个pointer
        int start = 0, end = 0, max = 0;
        // 记录two pointers的window 里面已经找到的character
        HashSet<Character> found = new HashSet<>();
        
        while(start < s.length() && end < s.length()) {
            // 移动末尾 end pointer，把没有repeat的character放入set中
            while(end < s.length() && !found.contains(s.charAt(end))) {
                max = Math.max(max, end - start + 1);
                found.add(s.charAt(end++));
            }
            
            // end处的char是重复的char，移动start pointer直到“移除”另外一个相同的s.charAt(end)
            if(end < s.length()) {
                while(s.charAt(start) != s.charAt(end)) {
                    found.remove(s.charAt(start));
                    start ++;
                }
                start ++;
            }
            end ++;
        }
        return max;
    }
}
```



