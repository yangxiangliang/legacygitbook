# 505. The Maze II

> There is a **ball **in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up**, **down**, **left **or **right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.
>
> Given the ball's **start position**, the **destination **and the **maze**, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of **empty spaces **traveled by the ball from the start position \(excluded\) to the destination \(included\). If the ball cannot stop at the destination, return -1.
>
> The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.
>
> **Example 1: **
>
> ```
> Input 1: a maze represented by a 2D array
>
> 0 0 1 0 0
> 0 0 0 0 0
> 0 0 0 1 0
> 1 1 0 1 1
> 0 0 0 0 0
>
> Input 2: start coordinate (rowStart, colStart) = (0, 4)
>
> Input 3: destination coordinate (rowDest, colDest) = (4, 4)
>
> Output: 12
>
> Explanation: One shortest way is : left -> down -> left -> down -> right -> down -> right.
>              The total distance is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
> ```
>
> **Example 2:**
>
> ```
> Input 1: a maze represented by a 2D array
>
> 0 0 1 0 0
> 0 0 0 0 0
> 0 0 0 1 0
> 1 1 0 1 1
> 0 0 0 0 0
>
> Input 2: start coordinate (rowStart, colStart) = (0, 4)
>
> Input 3: destination coordinate (rowDest, colDest) = (3, 2)
>
> Output: -1
>
> Explanation: There is no way for the ball to stop at the destination.
> ```
>
> **Note:**
>
> 1. There is only one ball and one destination in the maze.
> 2. Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
> 3. The given maze does not contain border \(like the red rectangle in the example pictures\), but you could assume the border of the maze are all walls.
> 4. The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.

### 思路

思路类似于题目 [10.2.39](/43-medium/10239-the-maze.md) 中的BFS方法，还加上 [Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) 的思想。注意在每一层BFS过程中，不能用boolean\[\]\[\] visited 来决定是否对该点附近的neighbors进行遍历，因为shortest path可能在第二次visit或则更多次的visit后才得出。处理的方法是用boolean\[\]\[\] length，来记录点\[i, j\]到start point的距离，如果该距离已经小于current point中的距离，则说明之前用current point更小的距离已经遍历过其周围的点了，可以直接skip current point，反之则 把current point的length值赋值给length\[i\]\[j\]，在该距离的基础上遍历其附近能让ball stop的点。

### Code

```java
public class Solution {
    class point {
        int x, y, l;
        public point(int x, int y, int l) {
            this.x = x;
            this.y = y;
            this.l = l;
        }
    }
    
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int m = maze.length, n = maze[0].length;
        int[][] diffs= new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int[][] length = new int[m][n];
        // 初始化 length map，length[i][j]表示点[i][j]到起点的距离
        for(int i = 0; i < m; i ++) 
            for(int j = 0; j < n; j ++) length[i][j] = Integer.MAX_VALUE;
        
        Queue<point> queue = new LinkedList();
        queue.add(new point(start[0], start[1], 0));
        while(!queue.isEmpty()) {
            point cur = queue.poll();
            if (length[cur.x][cur.y] <= cur.l) continue;
            length[cur.x][cur.y] = cur.l;
            
            for(int[] diff : diffs) {
                int x = cur.x, y = cur.y, l = cur.l;
                while (x >= 0 && x < m && y >= 0 && y < n && maze[x][y] == 0) {
                    x += diff[0];
                    y += diff[1];
                    l ++;
                }
                l --;
                // step back to validate point
                x -= diff[0];
                y -= diff[1];
                // add new starting point to queue
                queue.offer(new point(x, y, l));
            }
        }
        
        if(length[destination[0]][destination[1]] == Integer.MAX_VALUE) return -1;
        return length[destination[0]][destination[1]];
    }
}
```



