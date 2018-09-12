# 472. Concatenated Words

> Given a list of words \(**without duplicates**\), please write a program that returns all concatenated words in the given list of words.
>
> A concatenated word is defined as a string that is comprised entirely of at least two shorter words in the given array.

### 思路

这里可以构建Trie来做，应为和prefix有关。但是观察后，也可以直接用hashSet和DFS来处理，更简洁。

### Code

```java
public class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        List<String> rst = new ArrayList();
        HashSet<String> set = new HashSet();
        for (String word : words) set.add(word);
        for (String word: words) {
            if (helper(set, word, 0)) rst.add(word);
        }
        return rst;
    }

    public boolean helper(HashSet<String> set, String word, int count) {
        if (word == null || word.length() == 0) return count >= 2;
        // 注意 i 的取值范围
        for (int i = 1; i <= word.length(); i ++) {
            if (set.contains(word.substring(0, i))) {
                // Trick：因为concatenated word必须是由 >= 2个其他word组成
                // 所以这里用count表示用了多少子word来构造original word
                if (helper(set, word.substring(i), count+1)) return true;
            }
        }
        return false;
    }
}
```



