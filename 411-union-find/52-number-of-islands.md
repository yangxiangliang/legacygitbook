# 200. Number of Islands

> Given a 2d grid map of `'1'`s \(land\) and`'0'`s \(water\), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### 思路

想了union find的常规方法，感觉有点小题大做。可以直接DFS和一个count记录island的个数，最开始遇见‘1’的时候count ++，然后 “上，下，左，右” DFS中遇见“1”的时候需要把其set为“0”， 以免下次遇见时重复计算。

### Code

```java
public class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[0].length; j++) {
                if(grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count ++;
                }
            }
        }
        return count;
    }
    
    public void dfs(char[][] grid, int i, int j) {
        if(i < 0 || i >= grid.length || j < 0 | j >= grid[0].length) return;
        if(grid[i][j] == '0') return;
        grid[i][j] = '0';
        dfs(grid, i-1, j);
        dfs(grid, i + 1, j);
        dfs(grid, i, j -1);
        dfs(grid, i, j+1);
    }
}
```



