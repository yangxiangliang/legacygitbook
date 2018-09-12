# 694. Number of Distinct Islands

> Given a non-empty 2D array`grid`of 0's and 1's, an **island **is a group of`1`'s \(representing land\) connected 4-directionally \(horizontal or vertical.\) You may assume all four edges of the grid are surrounded by water.
>
> Count the number of **distinct **islands. An island is considered to be the same as another if and only if one island can be translated \(and not rotated or reflected\) to equal the other.
>
> **Example 1:**
>
> ```
> 11000
> 11000
> 00011
> 00011
>
> Given the above grid map, return 1.
> ```
>
> **Example 2:**
>
> ```
> 11011
> 10000
> 00001
> 11011
>
> Given the above grid map, return 3.
> ```
>
> Notice that:
>
> ```
> 11
> 1
> ```
>
> and
>
> ```
>  1
> 11
> ```
>
> are considered different island shapes, because we do not consider reflection / rotation.

### 思路

1. 找island 很容易，主要是怎么区分是不是相同的island。这里的trick是把找到island的每个坐标都相对于该island坐标的最上角的坐标作平移。这样得出的island的每个坐标都是相对于其左上角的相对位置，那么相同shape的island的各个坐标合计应该是相同的。
2. **注意**：有一个容易错的是计算 坐标\(i, j\) 相对于 \(x, y\) 的坐标值的时候，不能\(i - x\) \* columnLength + \(j - y\)。比如下面的情况：1......1,   01.......

        ...........,  100.....  第二个0相对于island最左上角的0 按照\(i - x\) \*columnLength + \(j - y\) 算出来是一样的，但是其相对位置不同。所以这里需要 \(i -x\) \* N \* columnLength + \(j-y\),  \(N &gt;= 2\) 来计算坐标平移后的相对坐标值才能保证每一个\(i, j\) 位置相对于 \(x, y\) 都有唯一的相对坐标值。

### Code

```java
class Solution {

    int[][] dirs = new int[][]{{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    int row;
    int col;
    public int numDistinctIslands(int[][] grid) {
        row = grid.length;
        col = grid[0].length;
        boolean[][] visited = new boolean[row][col];
        HashSet<HashSet<Integer>> rst = new HashSet();

        for(int i = 0; i < row; i ++) {
            for(int j = 0; j < col; j ++) {
                if(!visited[i][j] && grid[i][j] == 1) {
                    // tmp contains island中所有坐标相对于其左上角坐标的位置
                    HashSet<Integer> tmp = new HashSet();
                    // i, j 即就是该island的最左上角的坐标
                    dfs(grid, i, j, i, j, tmp, visited);
                    rst.add(tmp);
                }
            }
        }
        return rst.size();
    }

    private void dfs(int[][] grid, int i, int j, int sx, int sy, HashSet<Integer> path, boolean[][] visited) {
        if(i < 0 || i >= row || j < 0 || j >= col || visited[i][j] || grid[i][j] != 1) return;
        visited[i][j] = true;
        path.add(getIndex(i, j, sx, sy));

        for(int[] dir : dirs) dfs(grid, i + dir[0], j + dir[1], sx, sy, path, visited);
    }
    
    private int getIndex(int i, int j, int sx, int sy) {
        // 注意：这里不能 (i - sx) * col + (j - sy)，应该乘以2 才能保证每一个(sx, sy)才能有相对于(i, j) 唯一的坐标值
        return (i - sx) * 2 * col + (j - sy);
    }
}
```



