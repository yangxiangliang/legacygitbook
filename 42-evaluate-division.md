# 399. Evaluate Division

> Equations are given in the format `A / B = k`, where`A`and `B`are variables represented as strings, and `k`is a real number \(floating point number\). Given some queries, return the answers. If the answer does not exist, return `-1.0`.

### 思路

可以将此题中 _ A/B=K  _看作graph的edge，从vertex A到vertex B，该edge的weight是K。同时，有一个edge是从B--&gt;A，然后weight是 1/K。最后找X/Y的值，那么就是用DFS找是否有从X到Y的路径，然后将所有路径的weight相乘。

### Code

```java
public class Solution {
    class Edge {
        String val;
        double weight;
        public Edge(String val, double weight) {
            this.val = val;
            this.weight = weight;
        }
    }

    public double[] calcEquation(String[][] equations, double[] values, String[][] queries) {
        // 用hashMap记录graph的vertex和其对应的所有edge
        HashMap<String, List<Edge>> graph = new HashMap();
        // build the graph
        for (int i = 0; i < equations.length; i ++) {
            String v1 = equations[i][0];
            String v2 = equations[i][1];
            if (!graph.containsKey(v1)) graph.put(v1, new ArrayList<Edge>());
            if (!graph.containsKey(v2)) graph.put(v2, new ArrayList<Edge>());
            graph.get(v1).add(new Edge(v2, values[i]));
            graph.get(v2).add(new Edge(v1, 1 / values[i]));
        }
        double[] rst = new double[queries.length];
        for (int i = 0; i < queries.length; i ++) {
            // query edge的两边的vertex都不在graph中
            if(!graph.containsKey(queries[i][0]) || !graph.containsKey(queries[i][1])) rst[i] = -1.0;
            else rst[i] = calculateDFS(queries[i], new HashSet(), graph);
        }
        return rst;
    }

    public double calculateDFS(String[] query, HashSet<String> visited, HashMap<String, List<Edge>> graph) {
        String src = query[0];
        String dest = query[1];
        if (visited.contains(src)) return -1.0;
        if (src.equals(dest)) return 1.0;
        visited.add(src);
        for (Edge edge : graph.get(src)) {
            double tmp = calculateDFS(new String[]{edge.val, dest}, visited, graph);
            if (tmp > 0.0) return tmp*edge.weight;
        }
        // 这里不用 visited.remove(src)
        return -1.0;
    }
}
```



