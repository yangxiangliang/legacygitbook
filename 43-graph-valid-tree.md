# 261. Graph Valid Tree

> Given `n`nodes labeled from `0`to `n - 1`and a list of undirected edges \(each edge is a pair of nodes\), write a function to check whether these edges make up a valid tree.
>
> **Note: **you can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0, 1]`is the same as `[1, 0]`and thus will not appear together in `edges`.

### 思路

此题就是相当于用union find找**Undirected **graph中有没有cycle，valid tree中是没有cycle的，而且edges的数量只比vertices少1。

### Code

```java
public class Solution {
    // union find, detect cycle
    public boolean validTree(int n, int[][] edges) {
        int[] roots = new int[n];
        for (int i = 0; i < n; i++) roots[i] = i;
        // valid tree的edge数量应该是vertices数量 - 1
        if (edges.length != n - 1 ) return false;
        for (int[] pair : edges) {
            int root1 = findRoot(roots, pair[0]);
            int root2 = findRoot(roots, pair[1]);
            // 两个vertices恰好在一个set里，cycle存在
            if(root1 == root2) return false;
            roots[root2] = roots[root1];
        }
        return true;
    }

    public int findRoot(int[] roots, int p) {
        while(roots[p] != p) {
            roots[p] = roots[roots[p]];
            p = roots[p];
        }
        return p;
    }
}
```



