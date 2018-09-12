# 332. Reconstruct Itinerary

> Given a list of airline tickets represented by pairs of departure and arrival airports `[from, to]`, reconstruct the itinerary in order. All of the tickets belong to a man who departs from `JFK`. Thus, the itinerary must begin with `JFK`.
>
> **Note: **
>
> 1. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example,  the itinerary`["JFK", "LGA"]`has a smaller lexical order than`["JFK", "LGB"]`.
> 2. All airports are represented by three capital letters \(IATA code\).
> 3. You may assume all tickets form at least one valid itinerary.

### 思路

这里每个ticket中的departure，arrival airport codes相当于graph中的vertices，departure airport --&gt; arrival airport 相当于graph中的directed edge。题目中说"all tickets form at least one valid itinerary"，则说明从"JFK" vertice 开始总能找到一条DFS path可以遍历所有的directed edge。**注意Trick：（1）**可以用HashMap&lt;String, List&lt;String&gt;&gt; graph 来表示这个graph的vertice和edge，因为这里需要结果"has the smallest lexical order"，那么可以用**PriorityQueue** 来表示vertice的所有edge，即**HashMap&lt;String, PriorityQueue&lt;String&gt;&gt; graph** 来表示graph。而且还有一个好处，因为visit过的edge不能重复visit，所以visit一个edge后，直接将其从**PriorityQueue** poll出来即可，这样不用另外的数据结构来记录某个edge的visit情况；**（2）**这里用DFS先visit到最“深”处，最先完成DFS的vertice应该是在result中最后面的位置。所以对于**List&lt;String&gt; rst = new ArrayList\(\)，**结束某个vertice的DFS后，不用**rst.add\(current vertice\)**, 应该用 **rst.add\(0, current vertice\)**。\(这里画图便于理解\)



### Code

```java
public class Solution {
    // 从 JFK开始，遍历所有edge
    public List<String> findItinerary(String[][] tickets) {
        List<String> rst = new ArrayList();
        HashMap<String, PriorityQueue<String>> graph = new HashMap();
        for(String[] pair : tickets) {
            graph.putIfAbsent(pair[0], new PriorityQueue<String>());
            graph.get(pair[0]).add(pair[1]);
        }
        
        dfs(graph, rst, "JFK");
        return rst;
    }
    
    public void dfs(HashMap<String, PriorityQueue<String>> graph, List<String> rst, String departure) {
        PriorityQueue<String> arrivals = graph.get(departure);
        while(arrivals != null && !arrivals.isEmpty()) {
            dfs(graph, rst, arrivals.poll());
        }
        // 注意将current vertice 加入结果的顺序
        rst.add(0, departure);
    }
}
```



