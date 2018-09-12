# 642. Design Search Autocomplete System

> Design a search autocomplete system for a search engine. Users may input a sentence \(at least one word and end with a special character`'#'`\). For **each character **they type **except '\#'**, you need to return the **top 3 **historical hot sentences that have prefix the same as the part of sentence already typed. Here are the specific rules:
>
> 1. The hot degree for a sentence is defined as the number of times a user typed the exactly same sentence before.
> 2. The returned top 3 hot sentences should be sorted by hot degree \(The first is the hottest one\). If several sentences have the same degree of hot, you need to use ASCII-code order \(smaller one appears first\).
> 3. If less than 3 hot sentences exist, then just return as many as you can.
> 4. When the input is a special character, it means the sentence ends, and in this case, you need to return an empty list.
>
> Your job is to implement the following functions:
>
> The constructor function:
>
> `AutocompleteSystem(String[] sentences, int[] times):`This is the constructor. The input is **historical data**.`Sentences`is a string array consists of previously typed sentences.`Times`is the corresponding times a sentence has been typed. Your system should record these historical data.
>
> Now, the user wants to input a new sentence. The following function will provide the next character the user types:
>
> `List<String> input(char c):`The input`c`is the next character typed by the user. The character will only be lower-case letters \(`'a'`to`'z'`\), blank space \(`' '`\) or a special character \(`'#'`\). Also, the previously typed sentence should be recorded in your system. The output will be the **top 3 **historical hot sentences that have prefix the same as the part of sentence already typed.
>
> **Example:**
>
> **Operation: **AutocompleteSystem\(\["i love you", "island","ironman", "i love leetcode"\], \[5,3,2,2\]\)
>
> The system have already tracked down the following sentences and their corresponding times:
>
> `"i love you"`:`5`times  
> `"island"`:`3`times
>
> `"ironman"`:`2`times
>
> `"i love leetcode"`:`2`times
>
> Now, the user begins another search:
>
> **Operation:**input\('i'\)
>
> **Output:**
>
> \["i love you", "island","i love leetcode"\]
>
> **Explanation:**
>
> There are four sentences that have prefix `"i"`. Among them, "ironman" and "i love leetcode" have same hot degree. Since
>
> `' '`has ASCII code 32 and`'r'`has ASCII code 114, "i love leetcode" should be in front of "ironman". Also we only need to output top 3 hot sentences, so "ironman" will be ignored.
>
> **Operation: **input\(' '\)
>
> **Output: **\["i love you","i love leetcode"\]
>
> **Explanation: **
>
> There are only two sentences that have prefix`"i "`.
>
> **Operation: **input\('a'\)
>
> **Output:**\[\]
>
> **Explanation:**
>
> There are no sentences that have prefix`"i a"`.
>
> **Operation: **input\('\#'\)
>
> **Output:**\[\]
>
> **Explanation:**
>
> The user finished the input, the sentence`"i a"`should be saved as a historical sentence in system. And the following input will be counted as a new search.
>
> **Note：**
>
> 1. The input sentence will always start with a letter and end with '\#', and only one blank space will exist between two words.
> 2. The number of **complete sentences** that to be searched won't exceed 100. The length of each sentence including those in the historical data won't exceed 100.
>
> 3. Please use double-quote instead of single-quote when you write test cases even for a character input.
>
> 4. Please remember to **RESET** your class variables declared in class AutocompleteSystem, as static/class variables are
>
>    **persisted across multiple test cases**. Please see [here](https://leetcode.com/faq/#different-output) for more details.

### 思路

1. 首先这里需要根据prefix来找出所有可能的sentence，判断prefix的问题首先就想到了Trie来解决，会efficient。
2. 这里还需要记录每个sentence出现的次数，可以用一个HashMap来存，记录该sentence以及其出现频率的次数。
3. 最后找top 3 sentence，就是把该prefix 开头的所有sentence拿出来，比较其出现频率次数。有一个heap来找出top 3的sentence即可。
4. **这里有个trick**，为了更加efficient，不用每次给出prefix后，都遍历prefix后面的所有node来找出该prefix的所有sentence。可以在Trie中每个node存HashMap，map的key就是以到目前该node为prefix的sentence，value就是该sentence的出现频率。

### Code

```java
class AutocompleteSystem {

    class TrieNode {
        char val;
        HashMap<Character, TrieNode> children;
        // 记录到该node为prefix的所有sentence以及其频率count值
        HashMap<String, Integer> counts;
        boolean end;

        public TrieNode(char val) {
            this.val = val;
            children = new HashMap();
            counts = new HashMap();
        }
    }

    // 用于加入最后heap中，找出top 3的sentence
    class Result {
        String value;
        int count;

        public Result(String value, int count) {
            this.value = value;
            this.count = count;
        }
    }

    public void insert(String word, int count) {
        TrieNode node = root;
        for(char c : word.toCharArray()) {
            if(!node.children.containsKey(c)) node.children.put(c, new TrieNode(c));
            node = node.children.get(c);
            node.counts.put(word, node.counts.getOrDefault(word, 0) + count);
        }
        node.end = true;
    }

    TrieNode root;
    String pre; // pre表示之前已经input了的string
    public AutocompleteSystem(String[] sentences, int[] times) {
        pre = "";
        root = new TrieNode(' ');
        for(int i = 0; i < sentences.length ; i ++) {
            insert(sentences[i], times[i]);
        }
    }

    public List<String> input(char c) {
        List<String> rst = new ArrayList();
        if(c == '#') {
            insert(pre, 1);
            // reset pre string
            pre = "";
            return rst;
        }
        pre = pre + c;
        Queue<Result> minHeap = new PriorityQueue<Result>(new Comparator<Result>(){
            public int compare(Result a, Result b) {
                if(a.count != b.count) return a.count - b.count;
                return b.value.compareTo(a.value);
            }
        });

        TrieNode node = root;
        for(char child : pre.toCharArray()) {
            if(!node.children.containsKey(child)) return rst;
            node = node.children.get(child);
        }

        for(String s : node.counts.keySet()) {
            minHeap.add(new Result(s, node.counts.get(s)));
            if(minHeap.size() > 3) minHeap.poll();
        }
        while(minHeap.isEmpty() == false) rst.add(0, minHeap.poll().value);
        return rst;
    }
}

/**
 * Your AutocompleteSystem object will be instantiated and called as such:
 * AutocompleteSystem obj = new AutocompleteSystem(sentences, times);
 * List<String> param_1 = obj.input(c);
 */
```



