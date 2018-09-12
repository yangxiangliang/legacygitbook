# 692. Top K Frequent Words

> Given a non-empty list of words, return thekmost frequent elements.
>
> Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.
>
> **Example 1:**
>
> ```
> Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
> Output: ["i", "love"]
> Explanation: "i" and "love" are the two most frequent words.
>     Note that "i" comes before "love" due to a lower alphabetical order.
> ```
>
> **Example 2:**
>
> ```
> Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
> Output: ["the", "is", "sunny", "day"]
> Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
>     with the number of occurrence being 4, 3, 2 and 1 respectively.
> ```
>
> **Note:**
>
> 1. You may assume k is always valid, 1 &lt;= k &lt;= number of unique elements.
> 2. Input words contain only lowercase letters.
>
> **Follow up:**  
> 1. Try to solve it in O\(n log\(k\)\) time and O\(n\) extra space

### 思路

1. 找最大，最小的K frequent words，想到和找最大，最小的K elements 很像。想到可以用 priority queue 解题。**注意找“最大”问题的时候，用minHeap；找“最小”问题的时候，用maxHeap。因为比如找“最大”K问题，要维护一个size 为K的priority queue，当size &gt; k的时候，需要poll出当前queue中最小值，所以用minHeap.**
2. 这里需要一个东西记录word和其对应的frequency，而且要把这2个信息都放入priority queue中，可以用另外一个class包含这2个信息，也可以直接用Map.Entry&lt;String, Integer&gt;。
3. （**坑**）：priority queue中compare函数的构造，因为是min heap，所以frequency count越小的放越前面。如果frequency一样，要比较string，用str1.compareTo\(str2\)函数。根据题目要求，alphabetical order越小的因为排序越大，越排在queue的后面，这里容易错。

### Code

```java
class Solution {
    class Node {
        String val;
        int count;
        public Node(String val, int count) {
            this.val = val;
            this.count = count;
        }
    }
    
    public List<String> topKFrequent(String[] words, int k) {
        List<String> rst = new ArrayList();
        HashMap<String, Integer> counter = new HashMap();
        for(String word : words) counter.put(word, counter.getOrDefault(word, 0) + 1);
        
        Queue<Node> minHeap = new PriorityQueue<>(new Comparator<Node>(){
            public int compare(Node a, Node b) {
                // alphabetical order在前面的要放在minHeap的末尾，而不是前面
                if(a.count == b.count) return b.val.compareTo(a.val);
                return a.count - b.count;
            }
        });
        
        for(String word : counter.keySet()) {
            minHeap.add(new Node(word, counter.get(word)));
            if(minHeap.size() > k) minHeap.poll();
        }
        while(minHeap.size() > 0) rst.add(0, minHeap.poll().val);
        return rst;
    }
}
```



