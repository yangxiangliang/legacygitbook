# 737. Sentence Similarity II

> Given two sentences`words1, words2`\(each represented as an array of strings\), and a list of similar word pairs`pairs`, determine if two sentences are similar.
>
> For example,`words1 = ["great", "acting", "skills"]`and`words2 = ["fine", "drama", "talent"]`are similar, if the similar word pairs are`pairs = [["great", "good"], ["fine", "good"], ["acting","drama"], ["skills","talent"]]`.
>
> Note that the similarity relation **is **transitive. For example, if "great" and "good" are similar, and "fine" and "good" are similar, then "great" and "fine"**are similar**.
>
> Similarity is also symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.
>
> Also, a word is always similar with itself. For example, the sentences`words1 = ["great"], words2 = ["great"], pairs = []`are similar, even though there are no specified similar word pairs.
>
> Finally, sentences can only be similar if they have the same number of words. So a sentence like`words1 = ["great"]`can never be similar to`words2 = ["doubleplus","good"]`.

### 思路

1. 这个问题和 [Sentence Similarity](/10116-sentence-similarity.md) 的不同是similarity 是 transitive的。transitive的模型容易让人想到 union-find 模型，把所有similar的words都归类到同一个parent下面，每次检查2个word是否相同的时候，则检查其root是否一样即可。

### Code

```java
class Solution {
    public boolean areSentencesSimilarTwo(String[] words1, String[] words2, String[][] pairs) {
        if(words1.length != words2.length) return false;
        
        HashMap<String, String> roots = new HashMap();
        for(String[] pair : pairs) {
            // assign root as itself initially if not exists
            roots.putIfAbsent(pair[0], pair[0]);
            roots.putIfAbsent(pair[1], pair[1]);
            union(roots, pair[0], pair[1]);
        }
        
        for(int i = 0; i < words1.length; i ++) {
            if(words1[i].equals(words2[i]) || root(roots, words1[i]).equals(root(roots, words2[i]))) continue;
            return false;
        }
        return true;
    }
    
    private void union(HashMap<String, String> roots, String word1, String word2) {
        String root1 = root(roots, word1);
        String root2 = root(roots, word2);
        
        if(!root1.equals(root2)) {
            roots.put(root1, root2);
        }
    }
    
    private String root(HashMap<String, String> roots, String word) {
        // 如果roots map里面没有记录该word，则return word自己即可
        if(!roots.containsKey(word)) return word;
        while(!word.equals(roots.get(word))) word = roots.get(word);
        return word;
    }
}
```



