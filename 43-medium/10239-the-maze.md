# 490. The Maze

> There is a **ball **in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up**, **down**, **left **or **right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.
>
> Given the ball's **start position**, the **destination **and the **maze**, determine whether the ball could stop at the destination.
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
> Output: true
>
> Explanation: One possible way is : left -> down -> left -> down -> right -> down -> right.
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
> Output: false
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

用DFS的思路，从start point开始，能不能DFS到destination point，这里注意不能是visit到destination point就行，因为要看该ball能不能在destination point停下来。所以用boolean\[\]\[\] visit，不是表示所有visit过的点，而是便是visit到的能让ball stop下来的点。所以在1层DFS\(i, j\)中，\(i, j\)需要往left, right, upper, down走到没有wall为止，然后从终止点开始作下一层的DFS。同时，DFS function可以返回boolean，表示球从该点开始能不能到destination 停下来。

### Code 1\) DFS

```java
public class Solution {
    int m, n;

    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        m = maze.length;
        n = maze[0].length;
        boolean[][] visited = new boolean[m][n];
        return hasPath(maze, visited, start, destination);
    }

    public boolean hasPath(int[][] maze, boolean[][] visited, int[] start, int[] destination) {
        int x = start[0];
        int y = start[1];
        if(visited[x][y]) return false;

        visited[x][y] = true;
        // 此处的x, y 都是让ball可以停下来的点
        if(x == destination[0] && y == destination[1]) return true;

        // roll to left
        int i = y;
        while(i - 1 >= 0 && maze[x][i-1] != 1) i --;
        if(hasPath(maze, visited, new int[]{x, i}, destination)) return true;

        // roll to right
        i = y;
        while(i + 1 < n && maze[x][i+1] != 1) i ++;
        if(hasPath(maze, visited, new int[]{x, i}, destination)) return true;

        // roll to upper
        i = x;
        while(i - 1 >= 0 && maze[i-1][y] != 1) i--;
        if(hasPath(maze, visited, new int[]{i, y}, destination)) return true;

        // roll to down
        i = x;
        while(i + 1 < m && maze[i+1][y] != 1) i ++;
        if(hasPath(maze, visited, new int[]{i, y}, destination)) return true;

        return false;
    }
}
```

### Code 2\) BFS

```java
public class Solution {
    class point {
        int x, y;
        public point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        int m = maze.length, n = maze[0].length;
        int[][] diff= new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        boolean[][] visited = new boolean[m][n];

        Queue<point> queue = new LinkedList();
        queue.add(new point(start[0], start[1]));
        while(!queue.isEmpty()) {
            point cur = queue.poll();
            for(int i = 0; i < 4; i ++) {
                int x = cur.x + diff[i][0], y = cur.y + diff[i][1];
                while (x >= 0 && x < m && y >= 0 && y < n && maze[x][y] == 0) {
                    x += diff[i][0];
                    y += diff[i][1];
                }
                // step back to validate point
                x -= diff[i][0];
                y -= diff[i][1];
                if(visited[x][y]) continue;
                visited[x][y] = true;
                if(x == destination[0] && y == destination[1]) return true;
                queue.offer(new point(x, y));
            }
        }
        return false;
    }
}
```



