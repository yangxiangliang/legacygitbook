# 139. Word Break

> Given a **non-empty **string _s_ and a dictionary wordDict containing a list of **non-empty **words, determine if _s_ can be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.
>
> **For example,** given  
> s =`"leetcode"`,  
> dict =`["leet", "code"]`.
>
> Return true because`"leetcode"`can be segmented as`"leet code"`.

### 思路 1\)

最容易想到的思路，首先看s的substring有没有在wordDict中的，然后用recursion看另外一半substring是不是可以Word break的。用HashMap记录已经计算过的结果，来reduce running time。

### 复杂度

Time: O\(N^2\)，N 是输入string s的长度。这里的复杂度相当于可能要计算到所有cache的组合情况，因为cache中store的是原来string s.substring\(i, j\)的情况，\(i, j\)都是\(1, s.length\(\)\)之间的。所以一共有N^2 种组合，复杂度为O\(N^2\)

### Code 1\)

```java
public class Solution {
    HashMap<String, Boolean> cache = new HashMap();

    public boolean wordBreak(String s, List<String> wordDict) {
        HashSet<String> set = new HashSet();
        for(String tmp : wordDict) set.add(tmp);
        return helper(s, set);
    }

    public boolean helper(String s, HashSet<String> set) {
        if(cache.containsKey(s)) return cache.get(s);

        if(s.length() == 0) return true;
        if(set.contains(s)) return true;
        for(int i = 1; i <= s.length(); i ++) {
            if(set.contains(s.substring(0, i)) && helper(s.substring(i), set)) {
                cache.put(s, true);
                return true; 
            }
        }
        cache.put(s, false);
        return false;
    }
}
```

### 思路 2\)

观察题目，如果一个string是可以word break的，那么在其中某个点将string断开，可以认为前半段也肯定是word break的，然后后半段一定在wordDict中，所以更长的string是否可以word break的结果，是和较短的string是否可以word break的结果相关的，可以考虑用dynamic programming，用1个boolean\[s.length\(\) + 1\] map，然后map\[i\]表示长度为i的s.substring\(0, i\) 的 string是否可以word break。

### 复杂度

O\(N \* M\),  N: length of s, M: size of wordDict

### Code 2\)

```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] map = new boolean[s.length() + 1];
        map[0] = true;

        for(int i = 1; i <= s.length(); i ++) {
            if(map[i]) continue;
            for(String word : wordDict) {  // 这样也不用把input的list<String>加入到1个set中来lookup了
                if(i < word.length()) continue;
                if(map[i - word.length()] && s.substring(i-word.length(), i).equals(word)) {
                    map[i] = true;
                    break;
                }
            }
        }

        return map[s.length()];
    }
}
```



