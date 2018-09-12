# 734. Sentence Similarity

> Given two sentences`words1, words2`\(each represented as an array of strings\), and a list of similar word pairs`pairs`, determine if two sentences are similar.
>
> For example, "great acting skills" and "fine drama talent" are similar, if the similar word pairs are`pairs = [["great", "fine"], ["acting","drama"], ["skills","talent"]]`.
>
> Note that the similarity relation is not transitive. For example, if "great" and "fine" are similar, and "fine" and "good" are similar, "great" and "good" are **not **necessarily similar.
>
> However, similarity is symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.
>
> Also, a word is always similar with itself. For example, the sentences`words1 = ["great"], words2 = ["great"], pairs = []`are similar, even though there are no specified similar word pairs.
>
> Finally, sentences can only be similar if they have the same number of words. So a sentence like`words1 = ["great"]`can never be similar to`words2 = ["doubleplus","good"]`.

### 思路

1. 思路很容易，就用一个hashMap&lt;String, HashSet&lt;String&gt;&gt; 记录相似的pair信息即可。map的value 是 set 的原因是某个word 可能和好几个word是similar的。
2. 因为这里 similarity 是symmetric的，所以scan words1, words2 里面的word的时候；words1 中的word 或则 words2 中的word都有可能是上面map中的key，都要lookup 一下。

### Code

```java
class Solution {
    public boolean areSentencesSimilar(String[] words1, String[] words2, String[][] pairs) {
        if(words1.length != words2.length) return false;
        
        HashMap<String, HashSet<String>> map = new HashMap();
        for(String[] pair : pairs) {
            map.putIfAbsent(pair[0], new HashSet());
            map.get(pair[0]).add(pair[1]);
        }
        
        for(int i = 0; i < words1.length; i ++) {
            if(words1[i].equals(words2[i]) || map.containsKey(words1[i]) && map.get(words1[i]).contains(words2[i]) || map.containsKey(words2[i]) && map.get(words2[i]).contains(words1[i])) continue;
            return false;
        }
        return true;
    }
}
```



