# 140. Word Break II

> Given a **non-empty **string _s_ and a dictionary _wordDict_ containing a list of **non-empty **words, add spaces in _s_ to construct a sentence where each word is a valid dictionary word. You may assume the dictionary does not contain duplicate words.
>
> Return all such possible sentences.
>
> For example, given
>
> s =`"catsanddog"`,  
> dict =`["cat", "cats", "and", "sand", "dog"]`.
>
> A solution is`["cats and dog", "cat sand dog"]`.

### 思路

和题目[10.2.32](/43-medium/10232-word-break.md) 的思路类似，用DFS遍历input string _s_，当s.substring\(i, s.length\(\)\)在dictionary 中的时候，看s.substring\(0, i\)的break的情况。为了避免重复计算，用1个HashMap记录已经计算了的substring的break后的情况即可。

### 复杂度

Time：O\(n^3\)，n是string s的长度。要计算所有的cache map中的s.substring\(i, j\)的结果，因为\(i, j\)一共有\(n^2\)组合，计算每个组合的list&lt;String&gt;会take O\(n\)的time，所以最后得O\(n^3\)。

Space:  O\(n^3\)。因为cache 可能有O\(N^2\)个组合的s的substring，每个substring后面的List可能有N个，所以空间也是O\(n^3\)

### Code

```java
public class Solution {
    HashMap<String, List<String>> map = new HashMap();

    public List<String> wordBreak(String s, List<String> wordDict) {
        if(map.containsKey(s)) return map.get(s);

        HashSet<String> set = new HashSet(wordDict);
        List<String> rst = new ArrayList();
        for(int i = 0; i < s.length(); i ++) {
            String cur = s.substring(i);
            if(set.contains(cur)) {
                // i == 0的情况单独考虑，因为 i == 0的时候，不能进入下一个for循环
                // 但此时把s当做1个整体，是可以break的，应该加入结果中
                if(i == 0) rst.add(cur);
                for(String pre : wordBreak(s.substring(0, i), wordDict)) rst.add(pre + " " + cur);
            }
        }
        map.put(s, rst);
        return rst;
    }
}
```



