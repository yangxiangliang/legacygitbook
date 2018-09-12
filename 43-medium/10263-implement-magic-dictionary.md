# 676. Implement Magic Dictionary

> Implement a magic directory with`buildDict`, and`search`methods.
>
> For the method`buildDict`, you'll be given a list of non-repetitive words to build a dictionary.
>
> For the method`search`, you'll be given a word, and judge whether if you modify **exactly **one character into **another **character in this word, the modified word is in the dictionary you just built.
>
> **Example 1:**
>
> ```
> Input: buildDict(["hello", "leetcode"]), Output: Null
> Input: search("hello"), Output: False
> Input: search("hhllo"), Output: True
> Input: search("hell"), Output: False
> Input: search("leetcoded"), Output: False
> ```

### Code

```java
class MagicDictionary {

    HashSet<String> set;
    /** Initialize your data structure here. */
    public MagicDictionary() {
        set = new HashSet();
    }
    
    /** Build a dictionary through a list of words */
    public void buildDict(String[] dict) {
        for(String word : dict) set.add(word);
    }
    
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    public boolean search(String word) {
        for(int i = 0; i < word.length(); i ++) {
            char cur = word.charAt(i);
            for(char c = 'a'; c <= 'z'; c ++) {
                if(c != cur) {
                    String tmp = word.substring(0, i) + c + word.substring(i+1);
                    if(set.contains(tmp)) return true;
                }
            }
        }
        return false;
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dict);
 * boolean param_2 = obj.search(word);
 */
```



