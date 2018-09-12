# 317. Shortest Distance from All Buildings

> You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values **0**, **1 **or **2**, where:
>
> * Each **0** marks an empty land which you can pass by freely.
> * Each **1** marks a building which you cannot pass through.
> * Each **2** marks an obstacle which you cannot pass through.
>
> For example, given three buildings at`(0,0)`,`(0,4)`,`(2,2)`, and an obstacle at`(0,2)`:   
> 1 - 0 - 2 - 0 - 1
>
> \|     \|    \|    \|     \|
>
> 0 - 0 - 0 - 0 - 0
>
> \|    \|     \|    \|     \|
>
> 0 - 0 - 1 - 0 - 0
>
> The point `(1,2)`is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.
>
> **Note:**
>
> There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

### 思路

这里求**最短路径，容易想到的就是常用的 BFS的方法。**因为这里要求到所有building的最短路径之和。需要一个int\[\]\[\] distance记录每个empty\(0\) 点到每个路径的距离之和，而且对每个building\(1\) 点作BFS，计算其到所有能到达的empty\(0\) 点的距离，将其加在int\[\]\[\] distance里面。**这里还有trick： **就是有些empty 点可能无法达到所有的building，只能达到一部分building，这些点的distance里面的值是不能用的。所以还需要一个map int\[\]\[\] count记录该点能达到的building的个数，只有该个数等于 “总的building的个数” 时候，该点所对应的distance里面的值才是valid的。

### Code

```java
public class Solution {
    // 注意: 在DFS/BFS中，常见的处理方法，比较简洁的表示一个点左右neighbor的坐标关系
    int[] diffx = new int[]{0, 1, 0, -1}, diffy = new int[]{1, 0, -1, 0};
    
    class point {
        int x;
        int y;
        
        public point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    // 最短路径问题，想到用BFS解决
    public int shortestDistance(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] distance = new int[m][n];
        int[][] count = new int[m][n];
        int buildingNum = 0;
        
        for(int i = 0; i < m; i ++)
            for(int j = 0; j < n; j ++) {
                if(grid[i][j] == 1) {
                    dfs(new point(i, j), distance, count, grid, m, n);
                    buildingNum ++;
                }
            }
        
        int ans = Integer.MAX_VALUE;
        for(int i = 0; i < m; i ++)
            for(int j = 0; j < n; j ++) {
                if(grid[i][j] == 0 && count[i][j] == buildingNum) ans = Math.min(ans, distance[i][j]);
            }
        
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }
    
    public void dfs(point building, int[][] distance, int[][] count, int[][] grid, int m, int n) {
        Queue<point> queue = new LinkedList();
        queue.add(building);
        int level = 1;
        boolean[][] visited = new boolean[m][n];
        
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i ++) {
                point node = queue.poll();
                    
                for(int j = 0; j < 4; j ++) {
                    int x = node.x + diffx[j], y = node.y + diffy[j];
                    if(x < m && x >= 0 && y < n && y >= 0 && grid[x][y] == 0 && !visited[x][y]) {
                        distance[x][y] += level;
                        queue.add(new point(x, y));
                        count[x][y] ++;
                        visited[x][y] = true;
                    }
                }
            }
            level ++;
        }
    }
}
```



