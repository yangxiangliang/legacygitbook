# 269. Alien Dictionary

> There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of **non-empty **words from the dictionary, where **words are sorted lexicographically by the rules of this new language**. Derive the order of letters in this language.
>
> **Note:**
>
> 1. You may assume all letters are in lowercase.
> 2. You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.
> 3. If the order is invalid, return an empty string.
> 4. There may be multiple valid order of letters, return any one of them is fine.

### 思路

用Topological Sort的方法，最后如果有valid sort出的结果便是合理的排序，不然便返回empty string。**关键是怎么构建graph**： （1）理解题目意思，对于某一个Word内的character顺序，order是任意的，只是不同的words之间是lexicographically sorted的；（2）所以这里比较List的current Word和下一个Word的相同index的character，如果character相同，便skip过，遇见第一个不相同的character index n，则有 current word\[n\] --&gt; next word\[n\]，注意从此不再往之后的character比较了，他们之间order没有意义，因为2个Word的顺序只由第一个不同的character决定。

**e.g.** \["abch",  "abde"\] ，说明c应该排在d前面，h和e的排序无从得知。

### Code

```java
public class Solution {
    public String alienOrder(String[] words) {
        HashMap<Character, HashSet<Character>> graph = new HashMap();
        HashMap<Character, Integer> inDegrees = new HashMap();
        
        // build the graph
        for(int i = 0; i < words.length; i ++) {
            // Trick：flag去决定是否继续比较current Word和next word中的相同index的character，只比较第一个不同的char
            boolean compareNextWord = true;
            
            for(int j = 0; j < words[i].length(); j ++) {
                char char1 = words[i].charAt(j);
                graph.putIfAbsent(char1, new HashSet());
                inDegrees.putIfAbsent(char1, 0);
                
                if(i > 0 && j < words[i-1].length() && compareNextWord) {
                    char char2 = words[i-1].charAt(j);
                    if(char1 != char2) {
                        // edge应该是 char2 --> char1, 这里需要avoid duplicates 
                        if(graph.get(char2).contains(char1) == false) {
                            graph.get(char2).add(char1);
                            inDegrees.put(char1, inDegrees.get(char1) + 1);
                        }
                        // Trick，只比较2个Word的第一个不同的character即可
                        compareNextWord = false;
                    }
                }
            }
        }
        
        Queue<Character> queue = new LinkedList();
        for(char key : inDegrees.keySet()) {
            if(inDegrees.get(key) == 0) queue.add(key);
        }
        
        StringBuilder sb = new StringBuilder();
        while(!queue.isEmpty()) {
            int size = queue.size();
            
            for(int i = 0; i < size; i++){
                char cur = queue.poll();
                sb.append(cur);
                for(char neighbor : graph.get(cur)) {
                    inDegrees.put(neighbor, inDegrees.get(neighbor) - 1);
                    if(inDegrees.get(neighbor) == 0) queue.add(neighbor);
                }
            }
        }
        // 注意需要topological sort结果包含所有character，不然说明没有找到valid的topological sort
        return sb.length() == graph.size() ? sb.toString()  : "";
    }
}
```



