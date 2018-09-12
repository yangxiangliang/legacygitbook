# 286. Walls and Gates

> You are given a m x n 2D grid initialized with these three possible values.
>
> 1. -1 - A wall or an obstacle.
> 2. 0 - A gate.
> 3. INF - Infinity means an empty room. We use the value`2^31 - 1 = 2147483647`
>    to represent`INF`as you may assume that the distance to a gate is less than`2147483647`.
>
> Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with `INF`.

### 思路  1\)

这里是典型的求最短路径的题目，最短路径常考虑BFS解决。一开始想到的方法就是对每个gate的点开始，作BFS，然后看每个empty room的点到该gate的distance小于已有的值，则update其distance即可。

**问题:** 速度太慢，因为对于每个gate，都要BFS visit 一遍所有的点。

### Code 1\)

```java
public class Solution {
    int[] diffx = new int[]{0, 0, 1, -1};
    int[] diffy = new int[]{1, -1, 0, 0};

    public void wallsAndGates(int[][] rooms) {
        if(rooms == null || rooms.length == 0) return;

        int m = rooms.length, n = rooms[0].length;
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(rooms[i][j] == 0) bfs(rooms, i, j, m, n);
            }
        }
    }

    public void bfs(int[][] rooms, int i, int j, int m, int n) {
        Queue<int[]> queue = new LinkedList();
        queue.add(new int[]{i, j});
        int level = 0;
        boolean[][] visited = new boolean[m][n]; // 不要忘记 visited，避免重复visit

        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int num = 0; num < size; num ++) {
                int[] point = queue.poll();
                visited[point[0]][point[1]] = true;

                // 如果level 已经大于存在的值，说明该点周围有更近的gate，所以不用遍历其neighbor
                if(level > rooms[point[0]][point[1]]) continue;
                rooms[point[0]][point[1]] = level;

                for(int k = 0; k < 4; k ++) {
                    int x = point[0] + diffx[k], y = point[1] + diffy[k];
                    if(x >= 0 && x < m && y >= 0 && y < n && rooms[x][y] > 0 && !visited[x][y]) queue.add(new int[]{x, y});
                }
            }
            level ++;
        }
    }
}
```

### 思路 2）

\(**重要\)**   思路也是BFS，但是不用每次遇见gate时，都遍历所有的rooms。一开始将所有的gate加入queue中，然后开始BFS，如果遇见neighbor是empty room，则update 其 distance的值，然后加入queue中。因为这里的gate都在一开始加入queue中，那么gate的neighbor最早开始遍历的，每个empty room一定得到的就是到附近gate的最小值。

### 复杂度

Time O\(m \* n\), m 和 n 就是 2D matrix的维度，因为每个点在BFS时只会被visit一次。

### Code 2\)

```java
public class Solution {
    int[] diffx = new int[]{0, 0, 1, -1};
    int[] diffy = new int[]{1, -1, 0, 0};

    public void wallsAndGates(int[][] rooms) {
        if(rooms == null || rooms.length == 0) return;
        Queue<int[]> queue = new LinkedList();

        int m = rooms.length, n = rooms[0].length;
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(rooms[i][j] == 0) queue.add(new int[]{i, j}); // add all gates as start points in queue
            }
        }

        while(!queue.isEmpty()) {
            int size = queue.size();

            for(int num = 0; num < size; num ++) {
                int[] cur = queue.poll();
                int row = cur[0], col = cur[1];

                for(int i = 0; i < 4; i ++) {
                    int x = row + diffx[i], y = col + diffy[i];
                    if(x >= 0 && x < m && y >= 0 && y < n && rooms[x][y] == Integer.MAX_VALUE) {
                        //注意： 不用计level，(x, y)的值直接等于neighbor + 1 即可
                        rooms[x][y] = rooms[row][col] + 1;
                        queue.add(new int[]{x, y});
                    }
                }
            }
        }
    }
}
```



