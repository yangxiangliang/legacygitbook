# 425. Word Squares

> Given a set of words**\(without duplicates\)**, find all [word squares](https://en.wikipedia.org/wiki/Word_square) you can build from them.
>
> A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤k&lt; max\(numRows, numColumns\).
>
> For example, the word sequence`["ball","area","lead","lady"]`forms a word square because each word reads the same both horizontally and vertically.
>
> **Note: **
>
> 1. There are at least 1 and at most 1000 words.
> 2. All words will have the exact same length.
> 3. Word length is at least 1 and at most 5.
> 4. Each word contains only lowercase English alphabet **a-z**.

### 思路

仔细观察可得，如果要得到上面的word square，比如第一个word是 **ball** , 第二个word 的prefix必须是ball的第二个character，第三个word的prefix必须是1st word加上2nd word的第三个character，第四个word的prefix必须是前三个word的第四个character连在一起。所以想到可以先构建一个trie，方便找到以某个string为prefix的所有其他string。有一个**trick**是，这里在构建的trie的时候，可以有个小变化，在TrieNode中加入**List&lt;String&gt; startWith** ，表示在所有input words中，以Trie root到该TrieNode为止的string作为prefix的所有strings，方便后面查找以某个string为prefix的所有input strings。

### Code

```java
public class Solution {

    public class Trie {
        public class TrieNode {
            TrieNode[] children;
            // 和一般构建 TrieNode不同之处，小trick
            List<String> startWith;

            public TrieNode() {
                startWith = new ArrayList();
                children = new TrieNode[26];
            }
        }

        TrieNode root;
        /** Initialize your data structure here. */
        public Trie(String[] words) {
            root = new TrieNode();

            for (String word : words) {
                TrieNode cur = root;
                for (int i = 0; i < word.length(); i++) {
                    char c = word.charAt(i);
                    if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new TrieNode();
                    cur = cur.children[c - 'a'];
                    cur.startWith.add(word);
                }
            }
        }

        /** Returns if there is any word in the trie that starts with the given prefix. */
        public List<String> startsWith(String prefix) {
            TrieNode cur = root;
            List<String> rst = new ArrayList();
            for (int i = 0; i < prefix.length(); i++) {
                char c = prefix.charAt(i);
                if (cur.children[c - 'a'] == null) return rst;
                cur = cur.children[c - 'a'];
            }
            rst.addAll(cur.startWith);
            return rst;
        }
    }


    public List<List<String>> wordSquares(String[] words) {
        List<List<String>> rst = new ArrayList();
        Trie trie = new Trie(words);

        for (String word: words) {
            List<String> tmp = new ArrayList();
            tmp.add(word);
            searchHelper(1, tmp, rst, trie);
        }  
        return rst;
    }

    public void searchHelper(int index, List<String> preRows, List<List<String>> rst, Trie trie) {
        // terminate condition
        if (index == preRows.get(0).length()) {
            rst.add(new ArrayList(preRows));
            return;
        }

        StringBuilder prefix = new StringBuilder();
        for (String row : preRows) prefix.append(row.charAt(index));

        for (String row : trie.startsWith(prefix.toString())) {
            preRows.add(row);
            searchHelper(index+1, preRows, rst, trie);
            // 注意要remove 掉current row string，因为有可能有其他组合
            preRows.remove(preRows.size() - 1);
        }
    }
}
```



