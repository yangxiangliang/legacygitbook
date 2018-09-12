# 211. Add and Search Word - Data structure design

> Design a data structure that supports the following two operations:
>
> **void addWord\(word\)**
>
> **bool search\(word\)**
>
> search\(word\) can search a literal word or a regular expression string containing only letters **a-z** or **.** . A **.** means it can represent any one letter.
>
> **Note:** You may assume that all words are consist of lowercase letters **a-z**

### 思路

和之前**3.1.2**类似，不同的是**search\(word\)**中有**" . " **特殊符号，可能代表任何character。这里用一个**searchHelper** **function** 可以方便写recursion来解决

### Code

```java
class TrieNode {
    TrieNode[] children;
    boolean end;

    public TrieNode() {
        children = new TrieNode[26];
    }
}

public class WordDictionary {
    TrieNode root;

    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }

    /** Adds a word into the data structure. */
    public void addWord(String word) {
        TrieNode cur = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new TrieNode();
            cur = cur.children[c - 'a'];
        }
        cur.end = true;
    }

    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        return searchHelper(word, root);
    }

    private boolean searchHelper(String word, TrieNode cur) {
        // 避免 null pointer exception 
        if (cur == null) return false;

        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (c == '.') {
                for (TrieNode child: cur.children) {
                    if (searchHelper(word.substring(i+1), child)) return true;
                }
                return false;
            }
            if (cur.children[c - 'a'] == null) return false;
            cur = cur.children[c - 'a'];
        }
        return cur.end;
    }
}
```



