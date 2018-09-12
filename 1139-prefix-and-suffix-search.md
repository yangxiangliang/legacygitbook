# 745. Prefix and Suffix Search

> Given many`words`,`words[i]`has weight`i`.
>
> Design a class`WordFilter`that supports one function,`WordFilter.f(String prefix, String suffix)`. It will return the word with given`prefix`and`suffix`with maximum weight. If no word exists, return -1.
>
> **Examples:**
>
> ```
> Input:
> WordFilter(["apple"])
> WordFilter.f("a", "e") // returns 0
> WordFilter.f("b", "") // returns -1
> ```
>
> **Note:**
>
> 1. words consist of lowercase letters only.

### 思路

1. 一开始最 straightforward的方法就是建立2个Trie，一个按照加入的words的prefix建立，一个把words reverse过来按照suffix 建立Trie，每个TrieNode 还必须包含能search到该TrieNode的weight 数目。然后每次有prefix, suffix的时候，分别在2个Trie中search，会得到2个weights的集合，取2个集合中相同的元素中的最大weight即可。
2. 之后想有没有什么办法可以更简便，把2个Trie合并到一个trie来解决问题。这里关键是想怎么能把 suffix, prefix 和在一起来search。比如搜索apple，则可以input prefix\("a"\), suffix\("e"\);  prefix\("ap"\), suffix\("le"\)。看有没有办法把他们和在一起，成为一条string，在Trie中搜索。
3. 为了区分suffix 和 prefix，可以在其中加一个special character，比如 "\#"。对于apple，可以通过 “\#apple”， “e\#apple”, "le\#apple", "ple\#apple", "pple\#apple", "apple\#apple" 这些中的任何string的prefix来找到 apple。所以对于word \(“apple”\) 而言，可以把之前这些所有 string 都加入Trie中。**还需要注意的是**，加入新的char的时候，因为这里只需要return 最大的weight，每次更新该TrieNode的weight即可，只保留该TrieNode的最大的weight值即可。

### Code

```java
class WordFilter {

    class TrieNode {
        TrieNode[] children;
        char val;
        int weight;

        public TrieNode(char val) {
            this.val = val;
            children = new TrieNode[27]; // 26 for 26 lowercase letters, 1 for '#' separator
            weight = 0;
        } 
    }

    TrieNode root;
    public WordFilter(String[] words) {
        root = new TrieNode(' ');
        for(int k = 0; k < words.length; ++k) {
            String tmp = words[k] + "#";

            for(int i = 0; i < tmp.length(); ++i) {
                String insertWord = tmp.substring(i, tmp.length()) + words[k];
                TrieNode cur = root;

                for(int j = 0; j < insertWord.length(); ++j) {
                    char c = insertWord.charAt(j);
                    int index = (c == '#' ? 26 : c - 'a');
                    if(cur.children[index] == null) cur.children[index] = new TrieNode(c);
                    cur = cur.children[index];
                    cur.weight = k;
                }
            }
        }
    }

    public int f(String prefix, String suffix) {
        String target = suffix + "#" + prefix;

        TrieNode cur = root;
        for(char c : target.toCharArray()) {
            int index = (c == '#' ? 26 : c - 'a');
            if(cur.children[index] == null) return -1;
            cur = cur.children[index];
        }
        return cur.weight;
    }
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter obj = new WordFilter(words);
 * int param_1 = obj.f(prefix,suffix);
 */
```



