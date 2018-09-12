# 159. Longest Substring with At Most Two Distinct Characters

> Given a string, find the length of the longest substring T that contains at most 2 distinct characters.
>
> For example, Given s =`“eceba”`, T is "ece" which its length is 3.

### 思路 1\)

最容易想到的方法，但不是最佳方法。遍历所有的substring，用一个hashSet记录character，distinct characters的数量 &gt; 2的时候，便停止遍历包含该substring的其他substring。

### Code 1\)

```java
public class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        if(s.length() < 3) return s.length();
        int max = 2;
        for(int i = 0; i < s.length(); i ++) {
            HashSet<Character> set = new HashSet();
            set.add(s.charAt(i));
            for(int j = i + 1; j < s.length(); j ++) {
                set.add(s.charAt(j));
                if(set.size() <= 2) max = Math.max(j - i + 1, max);
                else break;  // 如果set.size() > 2，不用往后继续遍历了，剩下更长的string的distinct chars肯定多于2
            }
        }
        return max;
    }
}
```

### 思路 2）

和[题目10.3.1](/44-hard/1031-longest-substring-with-at-most-k-distinct-characters.md) 一模一样，只不过把K 值取 2 即可。就是维护一个sliding window，window的left, right bound所包含的string只有至多2个distinct characters。

### 复杂度

Time: O\(N\)，因为每个char只会被add入window，从window remove掉一次。

### Code 2\)

```java
public class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        int rst = 0;
        int[] count = new int[256];
        int distinct = 0;
        int left = 0;
        int right = 0;
        
        while(right < s.length()) {
            if(count[s.charAt(right)]++ == 0) distinct ++;
            
            while (distinct > 2) {
                if(--count[s.charAt(left++)] == 0) distinct --;
            }
            rst = Math.max(rst, right - left + 1);
            right ++;
        }
        return rst;
    }
}
```



