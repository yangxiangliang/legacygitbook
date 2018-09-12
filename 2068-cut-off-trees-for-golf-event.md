# 675. Cut Off Trees for Golf Event

> You are asked to cut off trees in a forest for a golf event. The forest is represented as a non-negative 2D map, in this map:
>
> 1. 0 represents the `obstacle`can't be reached.
> 2. `1`represents the`ground`can be walked through.
> 3. `The place with number bigger than 1`represents a`tree`can be walked through, and this positive number represents the tree's height.
>
> You are asked to cut off **all **the trees in this forest in the order of tree's height - always cut off the tree with lowest height first. And after cutting, the original place has the tree will become a grass \(value 1\).
>
> You will start from the point \(0, 0\) and you should output the minimum steps **you need to walk **to cut off all the trees. If you can't cut off all the trees, output -1 in that situation.
>
> You are guaranteed that no two`trees`have the same height and there is at least one tree needs to be cut off.
>
> **Example 1:**
>
> ```
> Input: 
> [
>  [1,2,3],
>  [0,0,4],
>  [7,6,5]
> ]
> Output: 6
> ```
>
> **Example 2:**
>
> ```
> Input: 
> [
>  [1,2,3],
>  [0,0,0],
>  [7,6,5]
> ]
> Output: -1
> ```
>
> **Example 3:**
>
> ```
> Input: 
> [
>  [2,3,4],
>  [0,0,5],
>  [8,7,6]
> ]
> Output: 6
> Explanation: You started from the point (0,0) and you can cut off the tree in (0,0) directly without walking.
> ```

### 思路

1. 因为这里需要从最短的树开始cur，然后到最高的树，所以一开始对每个位置根据其高度进行排序。根据排序后的位置去一个一个cut trees。
2. 从当前位置去下一个位置cut off trees的时候，之间走的最短path 则用BFS来计算即可，如果无法从当前点到下一个点去cut trees，则return -1 即可。

### Code

```java
class Solution {

    int[][] dirs = new int[][]{{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    int row;
    int col;
    public int cutOffTree(List<List<Integer>> forest) {
        row = forest.size();
        col = forest.get(0).size();

        List<int[]> lists = new ArrayList();
        for(int i = 0; i < row; i ++) {
            for(int j = 0; j < col; j ++) lists.add(new int[]{i, j});
        }

        Collections.sort(lists, new Comparator<int[]>(){
            public int compare(int[] a, int [] b) {
                return forest.get(a[0]).get(a[1]) - forest.get(b[0]).get(b[1]);
            }
        });

        int sx = 0, sy = 0;
        int ans = 0;
        for(int[] cur : lists) {
            if(forest.get(cur[0]).get(cur[1]) > 1) {
                int d = bfs(forest, sx, sy, cur[0], cur[1]);
                if(d < 0) return -1;
                sx = cur[0];
                sy = cur[1];
                ans += d;
            }
        }
        return ans;
    }

    private int bfs(List<List<Integer>> forest, int sx, int sy, int ex, int ey) {
        boolean[][] visited = new boolean[row][col];
        int count = 0;
        Queue<int[]> queue = new LinkedList();
        queue.offer(new int[]{sx, sy});

        visited[sx][sy] = true;
        while(!queue.isEmpty()) {
            int size =queue.size();
            for(int i = 0; i < size; i ++) {
                int[] cur = queue.poll();
                if(cur[0] == ex && cur[1] == ey) return count;

                for(int[] dir : dirs) {
                    int x = cur[0] + dir[0];
                    int y = cur[1] + dir[1];
                    if(x >= 0 && x < row && y >= 0 && y < col && !visited[x][y] && forest.get(x).get(y) > 0) {
                        visited[x][y] = true;
                        queue.offer(new int[]{x, y});
                    }
                }
            }
            count ++;
        }
        return -1;
    }
}
```



