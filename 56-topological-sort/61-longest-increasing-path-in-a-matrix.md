# 329. Longest Increasing Path in a Matrix

> Given an integer matrix, find the length of the longest increasing path.
>
> From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary \(i.e. wrap-around is not allowed\).

## 思路

观察matrix，发现一个\(i, j\)的longest increasing path等于该点附近的neighbor的longest increasing path + 1，当然前提是该neighbor的值大于该点。这里相当于一个递推，我们可以DFS + cache来解题，DFS\(i, j\) 表示遍历该点以及neighbor找到longest increasing path，同时用cache来记录DFS中得出的每个点对应的longest increasing path的长度，这样下次visit同一个点的时候，不用重复DFS，相当于一个visited map来记录该点是否DFS过。注意如果到达boundary，或则其neighbor值小于或等于\(i, j\)，则可以停止往下DFS该neighbor，因为找的path必须是 increasing的。

### Code

```java
public class Solution {
    // 表示遍历neighbor的4个方向，在matrix题目中常这样处理，让code 更clean
    int[][] neighbors = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0) return 0;
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] cache = new int[m][n];
        int max = 1;
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                max = Math.max(max, dfs(matrix, i, j, m, n, cache));
            }
        }
        return max;
    }
    
    public int dfs(int[][]matrix, int i, int j, int m, int n, int[][] cache) {
        // 如果cache中有值，则直接返回，说明该点被DFS visited过
        if(cache[i][j] != 0) return cache[i][j];
        int max = 1;
        for(int[] pair : neighbors) {
            // x, y是其neighbor的index值
            int x = pair[0] + i, y = pair[1] + j;
            if(x < 0 || x >= m || y < 0 || y >= n || matrix[x][y] <= matrix[i][j]) continue;
            max = Math.max(max, dfs(matrix, x, y, m, n, cache) + 1); 
        }
        cache[i][j] = max;
        return max;
    }
}
```



