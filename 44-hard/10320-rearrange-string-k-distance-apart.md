# 358. Rearrange String k Distance Apart

> Given a non-empty string **s **and an integer **k**, rearrange the string such that the same characters are at least distance **k **from each other.
>
> All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string`""`.
>
> **Example 1:**
>
> ```
> s = "aabbcc", k = 3
>
> Result: "abcabc"
>
> The same letters are at least distance 3 from each other.
> ```
>
> **Example 2:**
>
> ```
> s = "aaabc", k = 3 
>
> Answer: ""
>
> It is not possible to rearrange the string.
> ```
>
> **Example 3:**
>
> ```
> s = "aaadbbcc", k = 2
>
> Answer: "abacabcd"
>
> Another possible answer is: "abcabcda"
>
> The same letters are at least distance 2 from each other.
> ```

### 思路

* 首先观察怎么样才能让相同的character尽可能的分开，这里用到greedy的思想，对于出现次数最多的character，需要尽可能的将其"分散"，即build结果string的时候，先放入剩下出现次数最多的character。这里想到用1个maxHeap，里面记录每个character，以及其剩下的需要出现的次数，出现次数最多的排在前面。
* 但是注意1个问题，比如: "aaabc"，build结果string rst的时候，最开始因为"a"出现3次，所以 rst = "a"，然后"a“还需要出现2次，如果这时候把"a"放回heap，那么下一次取出的character还是"a"，放入结果string的时候，就变成''aa”连在一起的结果了。所以"a"从heap poll出来后不能着急放入heap中，至少要等又向结果中插入\(k-1\)个character后，才将其放回heap。那么这里还需要1个waiting queue，放从heap poll出来的元素，只有当waiting queue的size为k的时候，才将waiting queue中的第一个元素又加入heap中。
* 返回""的情况，就是在还没有build 完结果string的时候，heap已经为空，没有character可以poll出来了。

### 复杂度

O\(N \* log\(26\)\) = O\(N\)，因为都是lower case letter，heap的size最多为26，那么heap操作一次的complexity是log\(26\)

### Code

```java
public class Solution {
    class Node {
        char c;
        int count;
        public Node(char c, int count) {
            this.c = c;
            this.count = count;
        }
    }
    
    public String rearrangeString(String s, int k) {
        // 计算每个character以及在string中出现的次数
        HashMap<Character, Integer> count = new HashMap();
        for(char c : s.toCharArray()) {
            if(count.containsKey(c) == false) count.put(c, 0);
            count.put(c, count.get(c) + 1);
        }
        
        Queue<Node> heap = new PriorityQueue<Node>(new Comparator<Node>() {
            public int compare(Node n1, Node n2) {
                return n2.count - n1.count;
            }
        });
        for(char c : count.keySet()) heap.add(new Node(c, count.get(c)));
        
        char[] rst = new char[s.length()];
        // waiting queue，暂时存放从heap中poll出来的元素
        Queue<Node> waitQueue = new LinkedList();
        for(int i = 0; i < s.length(); i ++) {
            if(heap.size() == 0) return "";
            
            Node node = heap.poll();
            rst[i] = node.c;
            node.count --;
            waitQueue.add(node);
            
            if(waitQueue.size() < k) continue;
            else if(waitQueue.peek().count > 0) heap.add(waitQueue.poll());
            else waitQueue.poll();
        }
        return new String(rst);
    }
}
```



