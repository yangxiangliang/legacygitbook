# 127. Word Ladder

> Given two words \(_beginWord \_and \_endWord_\), and a dictionary's word list, find the length of shortest transformation sequence from _beginWord \_to \_endWord_, such that:
>
> 1. Only one letter can be changed at a time.
> 2. Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.
>
> **Note:**
>
> * Return 0 if there is no such transformation sequence.
> * All words have the same length.
> * All words contain only lowercase alphabetic characters.
> * You may assume no duplicates in the word list.
> * You may assume _beginWord_ and _endWord_ are non-empty and are not the same.
>
> **Example 1:**
>
> ```
> Input:
> beginWord = "hit",
> endWord = "cog",
> wordList = ["hot","dot","dog","lot","log","cog"]
>
> Output: 5
>
> Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
> return its length 5.
> ```
>
> **Example 2:**
>
> ```
> Input:
> beginWord = "hit"
> endWord = "cog"
> wordList = ["hot","dot","dog","lot","log"]
>
> Output: 0
>
> Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
> ```

### 思路

1. 经典的BFS题目，从begin Word开始，每次对其中1个位置的char换成新的string，如果新的string是在word dict当中并且没有被visit过的。那么加入BFS的queue当中。就这样“一层一层” BFS 直到最后走到end word 或则queue为空为止。
2. 注意加入queue中的Word只需要被加入其中一次。所以用一个set 记录已经被加入queue的word，避免重复加入。或则每加入一个Word，就将其从word dictionary 中remove掉即可。

### Code

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> set = new HashSet();
        for(String word : wordList) set.add(word);
        if(!set.contains(endWord)) return 0;

        int rst = 0;
        Queue<String> queue = new LinkedList();
        
        // begin word 加入queue中作为初始状态
        queue.add(beginWord);
        while(!queue.isEmpty()) {
            int size = queue.size();
            rst ++;
            
            for(int i = 0; i < size; i ++) {
                String cur = queue.poll();
                if(cur.equals(endWord)) return rst;
                
                for(int j = 0; j < cur.length(); j ++) {
                    for(char k = 'a'; k <= 'z'; k ++) {
                        if(k != cur.charAt(j)) {
                            String next = cur.substring(0, j) + k + cur.substring(j + 1);
                            if(set.contains(next)) {
                                queue.add(next);
                                // 把加入queue的string从dictionary中remove掉，避免被重复加入queue中
                                set.remove(next);
                            }
                        }
                    }
                }
            }
        }
        return 0;
    }
}
```



