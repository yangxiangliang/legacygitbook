# 648. Replace Words

> In English, we have a concept called`root`, which can be followed by some other words to form another longer word - let's call this word`successor`. For example, the root`an`, followed by`other`, which can form another word`another`.
>
> Now, given a dictionary consisting of many roots and a sentence. You need to replace all the`successor`in the sentence with the`root`forming it. If a`successor`has many`roots`can form it, replace it with the root with the shortest length.
>
> You need to output the sentence after the replacement.
>
> **Example 1:**
>
> ```
> Input: dict = ["cat", "bat", "rat"]
> sentence = "the cattle was rattled by the battery"
> Output: "the cat was rat by the bat"
> ```
>
> **Note:**
>
> 1. The input will only have lower-case letters.
> 2. 1 &lt;= dict words number &lt;= 1000
> 3. 1 &lt;= sentence words number &lt;= 1000
> 4. 1 &lt;= root length &lt;= 100
> 5. 1 &lt;= sentence words length &lt;= 1000

### 思路

1. 相当于是找一些词语的prefix，如果prefix在dict中，那么将其replace with prefix。则很容易想到efficient的方法就是用Trie。
2. 一开始用dict的words来build这个Trie，然后对于sentence中的每个词语一个一个在trie中search，如果其prefix在trie中存在，那么久把该prefix 和该Word replace即可。因为这里用shortest的prefix取代原Word，所以在Trie中search的时候，只用找到shortest的那个prefix 即可。

### Code

```java
class Solution {
    class TrieNode {
        char val;
        boolean end;
        HashMap<Character, TrieNode> children; // using hashMap for more efficient look up

        public TrieNode(char val) {
            this.val = val;
            children = new HashMap<>();
        }
    }

    class Trie {
        TrieNode root;

        public Trie() {
            root = new TrieNode(' ');
        }

        public void insert(String str) {
            TrieNode node = root;

            for(int i = 0; i < str.length(); i ++) {
                char key = str.charAt(i);
                if (node.children.containsKey(key)) node = node.children.get(key);
                else {
                    TrieNode newNode = new TrieNode(key);
                    node.children.put(key, newNode);
                    node = newNode;
                }
            }
            node.end = true;
        }

        public String hasRoot(String str) {
            TrieNode node = root;
            StringBuilder rootString = new StringBuilder();

            for (int i = 0; i < str.length(); i ++) {
                char key = str.charAt(i);
                if(node.end) return rootString.toString();
                if(!node.children.containsKey(key)) return "";
                rootString.append(key);
                node = node.children.get(key);
            }
            return "";
        }
    }
    
    public String replaceWords(List<String> dict, String sentence) {
        Trie trie = new Trie();
        for (String root : dict) trie.insert(root);
        StringBuilder rst = new StringBuilder();
        String[] words = sentence.split(" ");
        for(int i = 0; i < words.length; i ++) {
            String replaced = trie.hasRoot(words[i]);
            if(replaced.length() == 0) rst.append(words[i]);
            else rst.append(replaced);
            if(i != words.length - 1) rst.append(" ");
        }
        return rst.toString();
    }
}
```



