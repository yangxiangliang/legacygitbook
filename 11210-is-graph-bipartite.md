# 785. Is Graph Bipartite?

> Given an undirected `graph`, return`true`if and only if it is bipartite.
>
> Recall that a graph is _bipartite_  if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.
>
> The graph is given in the following form:`graph[i]`is a list of indexes`j`for which the edge between nodes`i`and`j`exists.  Each node is an integer between`0`and`graph.length - 1`.  There are no self edges or parallel edges:`graph[i]`does not contain`i`, and it doesn't contain any element twice.
>
> ```
> Example 1:
> Input: [[1,3], [0,2], [1,3], [0,2]]
> Output: true
> Explanation: 
> The graph looks like this:
> 0----1
> |    |
> |    |
> 3----2
> We can divide the vertices into two groups: {0, 2} and {1, 3}.
> ```
>
> ```
> Example 2:
> Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
> Output: false
> Explanation: 
> The graph looks like this:
> 0----1
> | \  |
> |  \ |
> 3----2
> We cannot find a way to divide the set of nodes into two independent subsets.
> ```
>
> **Note:**
>
> * _**graph**_** **will have length in range \[1, 100\].
> * _graph\[i\]_ will contain integers in range \[0, graph.length - 1\].
> * _graph\[i\]_ will not contain i or duplicate values.
> * The graph is undirected: if any element _j_ is in _graph\[i\]_, then _i_ will be in _graph\[i\]_.

### 思路

1. 这里考虑怎么表示用个set区分不同的node，一开始想到建立2个set。然后对graph进行遍历，对图形遍历通常就是DFS，BFS，下面采用的是DFS。遍历的时候如果把当前node放入其中一个set，那么其neighbor就必须放入另外一个set。但是在写code时候，感觉不好写。所以尝试有没有其他 的表示方法。
2. 其实可以不用set，就把每个node做一个Mark 标记即可。因为node就是0 到 graph.length - 1 之间的。可以就用一个int\[n\] marks 来给每个node做标记。这里可以用1标记为set1, 2 标记为 set2。然后 0 表示 还没有visit该node 即可。

### Code

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] marks = new int[n]; // 0 means unvisited, 1 means set_1, 2 means set_2
        
        for(int i = 0; i < n; i ++) {
            // 对于第一次DFS结束后，还没有visit的node，其实就是disconnected graph，所以其最开始标记1还是2没区别
            // 这里对于起始的node都标记为1
            if(marks[i] == 0 && !valid(graph, i, 1, marks)) return false;
        }
        return true;
    }
    
    private boolean valid(int[][] graph, int index, int mark, int[] marks) {
        if(marks[index] != 0) {
            // 如果已经被标记，只用检测被标记的和“应该”标记的Mark 是否矛盾即可
            return marks[index] == mark;
        }
        marks[index] = mark;
        for(int neighbor : graph[index]) {
           // Trick：标记只能是1或则2，所以当前index标记为 其中之一后，其neighbor 只能标记为另外1个，即(3-x)
           if(!valid(graph, neighbor, 3 - mark, marks)) return false;
        }
        return true;
    }
}
```



