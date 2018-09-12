# 291. Word Pattern II

> Given a`pattern`and a string`str`, find if`str`follows the same pattern.
>
> Here **follow **means a full match, such that there is a bijection between a letter in`pattern`and a **non-empty **substring in`str`.
>
> **Examples:**
>
> 1. pattern =`"abab"`, str =`"redblueredblue"`should return true.
> 2. pattern =`"aaaa"`, str =`"asdasdasdasd"`should return true.
> 3. pattern =`"aabb"`, str =`"xyzabcxzyabc"`should return false.
>
> **Notes:**  
> You may assume both`pattern`and`str`contains only lowercase letters.

### 思路

1. 题目拿到一开始容易没思路，因为没法找pattern string中每个character对应的substring。理论上其有可能和目标str中任何substring配对，需要一个一个去try，如果try到不对的时候，及时去掉该可能性，选择另外match的可能性match。In general的说，就是选择一种匹配方式尝试match下去，直到该条路径走不通，返回选择其他的路径。典型的**Backtracking 思路。**
2. 接下来想需要什么数据结构来处理，需要匹配pattern中的character和其有可能匹配的str中的substring的匹配关系，所以需要一个HashMap来记录该match关系。如果发现该种match关系进行不下去的时候，则需要从map中remove掉该种match关系，选择尝试其他的match关系来解题。
3. \(**坑\) ** 根据leetcode的test case，这里不同的pattern中的character不能匹配相同的string，所以还需要一个东西来记录已经被pattern的character中匹配了的string，那么其他的character不能与之匹配。**在面试中，这种条件需要向面试官问清楚再解答**

### 复杂度

Time: 这里worst case相当于pattern中第一个char要和str中从0开始substring一个一个比较，复杂度是N。然后pattern中第二个char需要和从str中index为1的substring一个一个比较是否是其对应pattern，复杂度是\(N-1\)。worst case复杂度是n\*\(n-1\)\*\(n-2\).. = O\(n!\)

Space: O\(N\)

### Code 1\)

```java
class Solution {
    public boolean wordPatternMatch(String pattern, String str) {
        Map<Character, String> map = new HashMap(); // record pattern char -> matched string
        Set<String> set = new HashSet(); // avoid duplicate matched string
        return isMatch(str, 0, pattern, 0, map, set);
    }

    private boolean isMatch(String str, int i, String pat, int j, Map<Character, String> map, Set<String> set) {
        if(i == str.length() && j == pat.length()) return true; // means perfectly matched
        // 表示一个string已经遍历，另外一个string还有剩余，则无法match成功
        if(i == str.length() || j == pat.length()) return false;

        if(map.containsKey(pat.charAt(j))) {
            String tmp = map.get(pat.charAt(j));
            if(tmp.length() + i > str.length() || !str.substring(i, i+tmp.length()).equals(tmp)) return false;
            return isMatch(str, i+tmp.length(), pat, j+1, map, set); // continue matching rest of string
        }

        // 表示这是一个新的pattern -> matched string，不知道pat.charAt(j)能和str中的什么substring match
        // 所以这里只有遍历str中从i开始到最末尾的substring，尝试其与pat.charAt(j)match，看能不能最后match成功
        for(int start = i+1; start <= str.length(); start++) {
            String match = str.substring(i, start);
            if(set.contains(match)) continue; // avoid duplicate matches

            map.put(pat.charAt(j), match);
            set.add(match);

            if(isMatch(str, start, pat, j+1, map, set)) return true;

            map.remove(pat.charAt(j));
            set.remove(match);
        }
        return false;
    }
}
```

### Code 2\)

思路和上面相同。就是在backtracking里面的recursion 的函数里，不用parse 该层遍历 pattern, str 的起始位置。只要每次recursion的时候parse入pattern, str的 substring 入下一层函数即可。

```java
class Solution {
    public boolean wordPatternMatch(String pattern, String str) {
        HashMap<Character, String> map = new HashMap();
        // 表示已经被mapped到的string，已经被mapped 到的string不能被pattern中不同的character map，即avoid duplicate mapping
        HashSet<String> mapped = new HashSet();
        return match(pattern, str, map, mapped);
    }
    
    private boolean match(String pattern, String str, HashMap<Character, String> map, HashSet<String> mapped) {
        if(pattern.length() > str.length()) return false;
        // 注意pattern string为空的情况
        if(pattern.length() == 0) return str.length() == 0;
        
        char cur = pattern.charAt(0);
        if(map.containsKey(cur)) {
            String tmp = map.get(cur);
            if(tmp.length() > str.length() || !tmp.equals(str.substring(0, tmp.length()))) {
                return false;
            } else {
                return match(pattern.substring(1), str.substring(tmp.length()), map, mapped);
            }
        }
        
        for(int j = 1; j <= str.length(); j ++) {
            String tmp = str.substring(0, j);
            // different pattern char does not match to same substring in str
            if(mapped.contains(tmp)) continue;
            
            map.put(cur, tmp);
            mapped.add(tmp);
            if(match(pattern.substring(1), str.substring(j), map, mapped)) {
                return true;
            }
            map.remove(cur);
            mapped.remove(tmp);
        }
        return false;
    }
}
```



