# 438. Find All Anagrams in a String

> Given a string **s **and a **non-empty **string **p**, find all the start indices of **p**'s anagrams in **s**.
>
> Strings consists of lowercase English letters only and the length of both strings **s **and **p **will not be larger than 20,100.
>
> The order of output does not matter.
>
> **Example 1**:
>
> ```
> Input:
> s: "cbaebabacd" p: "abc"
>
> Output:
> [0, 6]
>
> Explanation:
> The substring with start index = 0 is "cba", which is an anagram of "abc".
> The substring with start index = 6 is "bac", which is an anagram of "abc".
> ```
>
> **Example 2:**
>
> ```
> Input:
> s: "abab" p: "ab"
>
> Output:
> [0, 1, 2]
>
> Explanation:
> The substring with start index = 0 is "ab", which is an anagram of "ab".
> The substring with start index = 1 is "ba", which is an anagram of "ab".
> The substring with start index = 2 is "ab", which is an anagram of "ab".
> ```

### 思路

1. 如果需要 O\(N\) time complexity，那么典型的 two pointer 题目。维护一个长度为 p.length\(\) 的 window，看里面的string是否和P可以构成 anagrams。判断window里面的string是否构成anagram的时候，这里是用一个hashMap 存P中的所有character已经出现的频率，然后用另外一个 hashMap 存维护的window里面找到的character 以及其频率。除此之外，还需要一个counter来记录找到的character是否是target string P 中所需要的character，如果counter长度等于 P的长度，则说明找到一个符合要求的anagram了。

### Code

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> rst = new ArrayList();
        int s_leng = s.length(), p_leng = p.length();
        if(s_leng < p_leng) return rst;

        // 存 target string P 中的character和出现的频率数目
        HashMap<Character, Integer> target = new HashMap();
        for(int i = 0; i < p.length(); i ++) {
            target.put(p.charAt(i), target.getOrDefault(p.charAt(i), 0) + 1);
        }

        int count = 0;
        // 先看s string中第一个长度为p_leng的window里面包含的character以及其出现次数
        HashMap<Character, Integer> found = new HashMap();
        for(int i = 0; i < p_leng; ++i) {
            char c = s.charAt(i);
            if(target.containsKey(c) && found.getOrDefault(c, 0) < target.get(c)) count ++;
            found.put(c, found.getOrDefault(c, 0) + 1);
        }

        int left = 0, right = p_leng - 1;
        while(right < s_leng) {
            if(count == p_leng) rst.add(left);
            right ++; // 移动 sliding window 往右边一位
            if(right == s_leng) break;

            char left_char = s.charAt(left++);
            // 相当于 sliding window 右移动，把最左边的character移动出去了
            if(target.containsKey(left_char) && found.get(left_char) <= target.get(left_char)) count --;
            found.put(left_char, found.get(left_char) - 1);

            // 因为sliding window 右移动一位，把右边移动进来的character加入 found map中
            char right_char = s.charAt(right);
            if(found.getOrDefault(right_char, 0) < target.getOrDefault(right_char, 0)) count ++;
            found.put(right_char, found.getOrDefault(right_char, 0) + 1);
        }

        return rst;
    }
}
```

### Code 2\) 更简洁的写法

参考 two pointers 求 substring的[模板例子](https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems)

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> rst = new ArrayList();
        int start = 0, end = 0, match = p.length();
        int[] counts = new int[26];
        for(char c : p.toCharArray()) counts[c - 'a'] ++;
        
        while(end < s.length()) {
            if(counts[s.charAt(end++) - 'a'] -- > 0) match --;
            
            while(match == 0) {
                if(end - start == p.length()) rst.add(start);
                if(counts[s.charAt(start++) - 'a'] ++ == 0) match ++;
            }
        }
        return rst;
    }
}
```



