# 803. Bricks Falling When Hit

> We have a grid of 1s and 0s; the 1s in a cell represent bricks.  A brick will not drop if and only if it is directly connected to the top of the grid, or at least one of its \(4-way\) adjacent bricks will not drop.
>
> We will do some erasures sequentially. Each time we want to do the erasure at the location \(i, j\), the brick \(if it exists\) on that location will disappear, and then some other bricks may drop because of that erasure.
>
> Return an array representing the number of bricks that will drop after each erasure in sequence.
>
> ```
> Example 1:
> Input: 
> grid = [[1,0,0,0],[1,1,1,0]]
> hits = [[1,0]]
> Output: [2]
> Explanation: 
> If we erase the brick at (1, 0), the brick at (1, 1) and (1, 2) will drop. So we should return 2.
> ```
>
> ```
> Example 2:
> Input: 
> grid = [[1,0,0,0],[1,1,0,0]]
> hits = [[1,1],[1,0]]
> Output: [0,0]
> Explanation: 
> When we erase the brick at (1, 0), the brick at (1, 1) has already disappeared due to the last move. So each erasure will cause no bricks dropping.  Note that the erased brick (1, 0) will not be counted as a dropped brick.
> ```

### 思路

1. 看见题目想到用union find思想，所有brick 能和first row的brick连在一起则不用掉下来。
2. 正面思考，即一个一个去掉brick的思路，很难直接套用union find。那么可不可以 **反向思考**，hits的后面往前复原，即从最后开始一个一个加上去掉 的brick，每次把新加上的brick和周围的brick union在一起，算一个”不会掉的bricks“的总数，新增加的bricks的数量，就是去掉现在这个brick，会掉下去的bricks的数量。

### Code

```java
class Solution {
    class unionFind {
        int[] roots;
        int[] sizes;

        public unionFind(int n) {
            roots = new int[n];
            sizes = new int[n];

            for(int i = 0;i < n; i ++) {
                roots[i] = i;
                sizes[i] = 1;
            }
        }

        public int find(int i) {
            while(i != roots[i]) {
                roots[i] = roots[roots[i]];
                i = roots[i];
            }
            return i;
        }

        public void union(int i, int j) {
            int root1 = find(i);
            int root2 = find(j);
            if(root1 != root2) {
                roots[root1] = root2;
                sizes[root2] += sizes[root1];
            }
        }
    }

    int row;
    int col;
    public int[] hitBricks(int[][] grid, int[][] hits) {
        row = grid.length;
        col = grid[0].length;

        for(int[] hit : hits) {
            // mark hits的brick位置的值为2，然后一个一个把其值复原为1
            if(grid[hit[0]][hit[1]] == 1) grid[hit[0]][hit[1]] = 2;
        }

        unionFind uf = new unionFind(row * col + 1);
        for(int i = 0; i < row; ++i) {
            for(int j = 0; j < col; ++j) {
                if(grid[i][j] == 1) unionAround(i, j, grid, uf);
            }
        }

        int n = hits.length;
        int count = uf.sizes[uf.find(0)];
        int[] rst = new int[n];

        for(int i = n - 1; i >= 0; --i) {
            if(grid[hits[i][0]][hits[i][1]] == 2) {
                unionAround(hits[i][0], hits[i][1], grid, uf);
                grid[hits[i][0]][hits[i][1]] = 1;
            }
            
            // 把"0"作为最终 的root，连在这上面的nodes都是不会掉下来的
            int newCount = uf.sizes[uf.find(0)];
            // -1 是因为去掉手动加上的hits[i]位置的那个brick
            rst[i] = newCount > count ? newCount - count - 1 : 0;
            count = newCount;
        }
        return rst;
    }

    public void unionAround(int i, int j, int[][] grid, unionFind uf) {
        for(int[] dir : new int[][]{{-1, 0}, {1, 0}, {0, 1}, {0, -1}}) {
            int x = i + dir[0];
            int y = j + dir[1];
            
            // 把每个坐标(i, j) 值都偏移offset 1，这样把 roots[] 中index为0的位置留出来，把其作为“最终的root”
            if(x < row && x >= 0 && y < col && y >= 0 && grid[x][y] == 1) uf.union(i * col + j + 1, x * col + y + 1);
        }

        // use 0 as pivot to connect all elements in row 0
        if(i == 0) uf.union(i * col + j + 1, 0);
    }
}
```



