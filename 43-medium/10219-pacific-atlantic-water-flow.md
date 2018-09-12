# 417. Pacific Atlantic Water Flow

> Given an`m x n`matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.
>
> Water can only flow in four directions \(up, down, left, or right\) from a cell to another one with height equal or lower.
>
> Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.
>
> **Note: **
>
> 1. The order of returned grid coordinates does not matter.
> 2. Both _m_ and _n_ are less than 150.

### 思路

容易想到DFS处理，因为这里需要同时达到 Pacific 和 Atlantic 两条边，合在一起处理比较乱和复杂。我们可以分开处理，用2个map, boolean\[\]\[\] pacific, boolean\[\]\[\] atlantic，分别从Pacific 和 Atlantic的边界开始往里 DFS，注意这里是逆向 DFS，即往"高处" DFS。到达的point则Mark其对应的point为 true，表示能达到 Pacific 或则 Atlantic。最后遍历所有点，pacific\[i\]\[j\] 和 atlantic\[i\]\[j\] 同时为 true时便是符合要求的点。

### 注意

* 这里将 “到达 Pacific” 和 “到达 Atlantic” 分开处理，分开用Map 记录比较简洁和容易理解。
* **采用逆向思维，从边界往里DFS 找到能达到的点。比 一般的思维：从每个点开始DFS，看能不能到达 边界；更容易implement**

### Code

```java
public class Solution {
    int[] diffx = new int[]{0, 0, 1, -1}, diffy = new int[]{1, -1, 0, 0};
    boolean[][] pacific;
    boolean[][] atlantic;
    
    public List<int[]> pacificAtlantic(int[][] matrix) {
        List<int[]> rst = new ArrayList();
        if(matrix == null || matrix.length == 0) return rst;
        
        int m = matrix.length, n = matrix[0].length;
        pacific = new boolean[m][n];
        atlantic = new boolean[m][n];
        
        for(int i = 0; i < m; i ++) {
            dfs(matrix, pacific, i, 0, m, n); // 从 Pacific 的边界开始
            dfs(matrix, atlantic, i, n-1, m, n); // 从 Atlantic边界开始
        }
        
        for(int i = 0; i < n; i ++) {
            dfs(matrix, pacific, 0, i, m, n); // 从 Pacific 的边界开始
            dfs(matrix, atlantic, m-1, i, m, n); // 从 Atlantic边界开始
        }
        
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(pacific[i][j] && atlantic[i][j]) rst.add(new int[]{i, j});
            }
        }
        return rst;
    }
    
    public void dfs (int[][] matrix, boolean[][] visited, int i, int j, int m, int n) {
        if(visited[i][j]) return;
        
        visited[i][j] = true;
        for(int k = 0; k < 4; k ++) {
            int x = i + diffx[k], y = j + diffy[k];
            // 逆向DFS，所以 条件为 >=，而不是 <=
            if(x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] >= matrix[i][j]) {
                dfs(matrix, visited, x, y, m, n);  
            }
        }
    }
}
```



