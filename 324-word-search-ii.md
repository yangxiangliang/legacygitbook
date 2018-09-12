# 212. Word Search II

> Given a 2D board and a list of words from the dictionary, find all words in the board.
>
> Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
>
> **Note:**
>
> You may assume that all inputs are consist of lowercase letters`a-z`.

### 思路

因为在board中一个一个找word，容易想到要用DFS，而且用Trie作prefix查找更快，所以想到DFS和Trie相结合。

### Code 1\)

```java
public class Solution {
    public class TrieNode {
        TrieNode[] children;
        boolean end;

        public TrieNode() {
            children = new TrieNode[26];
        }
    }

    public class Trie {
        TrieNode root;

        public Trie() {
            root = new TrieNode();
        }

        public void insert(String word) {
            TrieNode cur = root;
            for (int i = 0; i < word.length(); i++) {
                char c = word.charAt(i);
                if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new TrieNode();
                cur = cur.children[c - 'a'];
            }
            cur.end = true;
        }

        public boolean search(String word) {
            TrieNode cur = root;
            for (int i = 0; i < word.length(); i++) {
                char c = word.charAt(i);
                if (cur.children[c - 'a'] == null) return false;
                cur = cur.children[c - 'a'];
            }
            return cur.end;
        }

        public boolean startsWith(String prefix) {
            TrieNode cur = root;
            for (int i = 0; i < prefix.length(); i++) {
                char c = prefix.charAt(i);
                if (cur.children[c - 'a'] == null) return false;
                cur = cur.children[c - 'a'];
            }
            return true;
        }
    }
    // 上面是implement trie的基本方法
    
    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        // 用HashSet是为了结果中avoid duplicates
        HashSet<String> rst = new HashSet();
        int height = board.length;
        int width = board[0].length;

        // 用visited 矩阵记录哪些cell visit过，因为同一个cell在一个word中只能visit一次        
        boolean[][] visited = new boolean[height][width];
        for (String word : words) trie.insert(word);
        for (int i = 0; i < height; i ++) {
            for (int j = 0; j < width; j ++) {
                dfs(board, rst, i, j, "", trie, visited);
            }
        }
        return new ArrayList(rst);
    }
    public void dfs (char[][] board, HashSet<String> rst, int i, int j, String prefix, Trie trie, boolean[][] visited) {
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length) return;
        if (visited[i][j]) return; //如果已经visit了该cell，直接return
        prefix += board[i][j];
        
        if (!trie.startsWith(prefix)) return;
        if (trie.search(prefix)) rst.add(prefix);
     
        visited[i][j] = true;
        dfs(board, rst, i+1, j, prefix, trie, visited);
        dfs(board, rst, i-1, j, prefix, trie, visited);
        dfs(board, rst, i, j+1, prefix, trie, visited);
        dfs(board, rst, i, j-1, prefix, trie, visited);
        visited[i][j] = false; // 注意：最后结束时，要将该cell reset回没有visit过的情况
    }
}
```

### Code 2\)

上面的code太复杂了，有些东西可以省略掉。

1. 不需要Trie的 **search**, 和 **startsWith**的functions，只需要Trie的root TrieNode即可，一层一层往下找。
2. 也不需要TrieNode 中的 **end** boolean，可以将其换成 word，表示从root 到该TrieNode时，可以组成word string。
3. 不需要boolean\[\]\[\]  visited，在原来board上修改成一个 不在**a-z** 范围内的char即可，注意最后也需要reset 回原来char。
4. 在DFS中不需要一直Parse prefix，只要TrieNode的word 不为null，便将TrieNode的word 加入结果即可。**注意:** 此时需要将TrieNode的word设为null，是为了avoid duplication，因为该word已经加入结果。

```java
public class Solution {
    public class TrieNode {
        TrieNode[] children;
        String word;

        public TrieNode() {
            children = new TrieNode[26];
        }
    }
    
    public TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode p = root;
            for (char c : word.toCharArray()) {
                int i = c - 'a';
                if (p.children[i] == null) p.children[i] = new TrieNode();
                p = p.children[i];
            }
            p.word = word;
        }
        return root;
    }
    
    public List<String> findWords(char[][] board, String[] words) {
        TrieNode root = buildTrie(words);
        List<String> rst = new ArrayList();

        for (int i = 0; i < board.length; i ++) {
            for (int j = 0; j < board[0].length; j ++) {
                dfs(board, rst, i, j, root);
            }
        }
        return rst;
    }
    public void dfs (char[][] board, List<String> rst, int i, int j, TrieNode node) {
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length) return;
        
        char c = board[i][j];
        // 说明该cell被visited 过，或则往下没有对应的child了
        if (c == 'X' || node.children[c - 'a'] == null) return;
        
        node = node.children[c - 'a'];
        if (node.word != null) {
            rst.add(node.word);
            node.word = null; // avoid duplicates
        }
     
        board[i][j] = 'X';
        dfs(board, rst, i+1, j, node);
        dfs(board, rst, i-1, j, node);
        dfs(board, rst, i, j+1, node);
        dfs(board, rst, i, j-1, node);
        board[i][j] = c;
    }
}
```



