# 444. Sequence Reconstruction

> Check whether the original sequence `org`can be uniquely reconstructed from the sequences in `seqs`.  The `org`sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 10^4. Reconstruction means building a shortest common supersequence of the sequences in `seqs`\(i.e., a shortest sequence so that all sequences in `seqs`are subsequences of it\). Determine whether there is only one sequence that can be reconstructed from `seqs`and it is the `org`sequence.
>
> **Example1:**
>
> ```
> Input:
> org: [1,2,3], seqs: [[1,2],[1,3]]
>
> Output:
> false
>
> Explanation:
> [1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.
> ```
>
> **Example2:**
>
> ```
> Input:
> org: [1,2,3], seqs: [[1,2],[1,3],[2,3]]
>
> Output:
> true
>
> Explanation:
> The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].
> ```

### 思路

这个题目和Topological Sort的思想类似，可以把seqs 转换为一个一个的directed edge，求其topological sort，如果结果有唯一的topological sort，而且与 input **org** 相同，则return **True**，反之，return **False**。**Trick 注意：（1）**在转换 seqs 为directed edge时，比如遇见\[1, 2, 3, 4\]，只用构造edge 1 -&gt; 2, 2 -&gt; 3, 3 -&gt; 4 即可，不用再构造 1 -&gt; 3 edge，因为 **"1-&gt;2, 2-&gt;3"** 2个edge已经包含了**"1-&gt;3"**

edge。

### Code

```java
public class Solution {
    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
        // 因为test case中 含有很大的number，可能超出org.length，所以这里用HashMap表示graph，不用List<> 表示
        HashMap<Integer, HashSet<Integer>> graph = new HashMap();
        HashMap<Integer, Integer> inDegrees = new HashMap(); // 记录vertice indgree的个数
        // build graph
        for(List<Integer> neighbors : seqs) {
            for(int i = 0; i < neighbors.size(); i ++) {
                graph.putIfAbsent(neighbors.get(i), new HashSet());
                inDegrees.putIfAbsent(neighbors.get(i), 0);
                if (i > 0) {
                    graph.putIfAbsent(neighbors.get(i-1), new HashSet());
                    inDegrees.putIfAbsent(neighbors.get(i-1), 0);

                    // seqs 中可能有duplicates,这里avoid duplicate 也计算到inDegrees的个数中
                    if(graph.get(neighbors.get(i-1)).contains(neighbors.get(i)) == false) {
                        graph.get(neighbors.get(i-1)).add(neighbors.get(i));
                        inDegrees.put(neighbors.get(i), inDegrees.get(neighbors.get(i)) + 1);
                    }
                }
            }
        }
        
        // 如果graph的vertice的size和org length不同，那么不可能是唯一的topological sort
        if(graph.size() != org.length) return false;
        
        Queue<Integer> queue = new LinkedList();
        for(int key : inDegrees.keySet()) {
            if(inDegrees.get(key) == 0) queue.add(key);
        }
        
        int index = 0;
        while(!queue.isEmpty()) {
            // 如果queue.size > 1，因为该queue中的元素的顺序都可以组成topological sort，则说明没有唯一的拓扑排序
            if(queue.size() > 1) return false;
            int cur = queue.poll();
            if(index == org.length || org[index++] != cur) return false;
            for(int neighbor : graph.get(cur)) {
                inDegrees.put(neighbor, inDegrees.get(neighbor) - 1);
                if(inDegrees.get(neighbor) == 0) queue.add(neighbor);
            }
        }
        // 看index是不是check完了org 中的所有元素
        return index == org.length;
    }
}
```



