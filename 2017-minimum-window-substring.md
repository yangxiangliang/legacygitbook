# 76. Minimum Window Substring

> Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O\(n\).
>
> For example,  
> **S **=`"ADOBECODEBANC"`  
> **T **=`"ABC"`
>
> Minimum window is`"BANC"`.
>
> **Note:**  
> If there is no such window in S that covers all characters in T, return the empty string`""`.
>
> If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.

### 思路

1. 这里为了知道 S string的某个窗口中是否包含了T中的所有character，那么需要一个map记录T中的character和其出现的次数。
2. 需要求window中的substring的问题，往往想到“双针模型”，即2个pointer，一个left pointer\(begin pointer\)，一个right pointer\(end pointer\)。一开始left, right pointers都在最左边，先移动right pointer到一个能包含所有T的index处，这样得到一个初始化的substring可以包含T。然后移动left pointer，如果left pointer移动出了一个T中的character，那么这时候又需要移动right pointer到一个index，该index处的character刚好是被left pointer移出去的character，这时候又得到一个符合要求的substring，如此往复直到right pointer移动到最右端，取整个过程中最短的substring即可。

### 复杂度

Time: O\(N\)，N是S的长度。因为是“双针模型”，每次left, right pointers都在往右边移动，最多移动到S.length\(\)处，所以是O\(N\)。

Space: O\(1\)。可以认为输入的字母就是ASSIC code，或则就是letter，所以用HashMap存的key的数量是有限的，constant级别的。或则可以用int\[128\] 来存，所以空间complexity就是constant级别的。

### Code

```java
class Solution {
    public String minWindow(String s, String t) {
        if(t.length() == 0) return "";
        // 记录在 s 中current window中发现的characters      
        HashMap<Character, Integer> found = new HashMap();
        // 记录 target string，即 T 中的characters的数量
        HashMap<Character, Integer> count = new HashMap();
        for(char c : t.toCharArray()) count.put(c, count.getOrDefault(c, 0) + 1);
        // 上面count map的copy，后面要用，因为下面找initial的left, right pointer时，count map被清空了，所以这里copy下
        HashMap<Character, Integer> target = new HashMap(count);

        int i = 0, j = 0; // i is left pointer, j is right pointer
        for(; j < s.length(); j ++) {
            char c = s.charAt(j);
            found.put(c, found.getOrDefault(c, 0) + 1);
            if(count.containsKey(c)) {
                count.put(c, count.get(c) - 1);
                if(count.get(c) == 0) count.remove(c);
            }
            if(count.size() == 0) break;
        }
        if(j == s.length()) return ""; // s cannot cover t string anyway

        String rst = s.substring(i, j+1);
        while(j < s.length()) {
            //尝试移除最左边的character
            char left = s.charAt(i);
            i ++;
            found.put(left, found.get(left) - 1);

            // 如果移除的最左边的character，刚好是需要的target中的character，那么尝试移动右边的pointer j
            // 去找到刚才移除的左边 的character
            if(target.containsKey(left) && found.get(left) < target.get(left)) {
                j ++;
                for(; j < s.length(); j ++) {
                    char tmp = s.charAt(j);
                    found.put(tmp, found.getOrDefault(tmp, 0) + 1);
                    if(left == tmp) break;
                }
            }

            // 此时window [i, j] 则是符合要求的substring
            if(j < s.length() && (j - i + 1) < rst.length()) rst = s.substring(i, j+1);
        }
        return rst;
    }
}
```

### 思路 2）

思路和上面，也是用 two pointer的思想。但是写法简答很多。可以套用 \[这里的\]\([https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems](https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems)\) two pointer求substring的模板。

### Code 2\)

```java
class Solution {
    public String minWindow(String s, String t) {
        // 假设输入的char是ascii的，则其对应的int的range在128范围内
        // 所以这里用length为128的数组记录char出现的频率即可
        int[] counts = new int[128];
        for(char c : t.toCharArray()) counts[c] ++;
        int match = t.length(), start = 0, end = 0;
        String rst = "";

        while(end < s.length()) {
            // 表示找到了t中的char，所以匹配上的match - 1
            if(counts[s.charAt(end++)]-- > 0) match --;

            while(match == 0) {
                if(rst.length() == 0 || end - start < rst.length()) rst = s.substring(start, end);
                // 如果char不是t中的，那么counts一定都是小于0的；如果 counts[start_char] 等于0
                // 说明遇见了t中的char，所以match + 1, 然后让current substring invalid，再去找下一个valid substring
                if(counts[s.charAt(start++)]++ == 0) match ++;
            }
        }
        return rst;
    }
}
```



