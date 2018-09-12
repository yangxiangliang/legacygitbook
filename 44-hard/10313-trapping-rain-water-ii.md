# 407. Trapping Rain Water II

> Given an `m x n`matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.
>
> **Note:**
>
> Both _m_ and \_n \_are less than 110. The height of each unit cell is greater than 0 and is less than 20,000.
>
> **Example: **
>
> ```
> Given the following 3x6 height map:
> [
>   [1,4,3,1,3,2],
>   [3,2,1,3,2,4],
>   [2,3,3,2,3,1]
> ]
>
> Return 4.
> ```
>
> ![](/assets/rainwater_empty.png)
>
> The above image represents the elevation map `[[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]`before the rain.
>
> ![](/assets/rainwater_fill.png)
>
> After the rain, water are trapped between the blocks. The total volume of water trapped is 4.

### 思路

这道题目感觉和前面[10.3.12](/44-hard/10312-trapping-rain-water.md) 有类似的，但是前面题目是平面1维的，这里是立体2维的。想到决定某一点的存水量的因素是什么，其实也是和四周的高度有关系，而且也是由4周高度中最短的那个决定的，即"木桶原理"。因为最外围一圈的点是不可能存水的，所以可以先从最外围一圈的点向里面开始遍历，因为1个点的存水量是由周围的最短的决定的，所以应该先从高度较低的点，遍历其4个neighbors，如果neighbor没有被visit过，那么计算其存水量，加到结果中。因为从高度较小的点开始visit，所以想到用minHeap来做BFS，每次总是取出高度最小的点，visit过的点再加入minHeap中，直到所有的点都被visit过。

### 注意

Tricky的地方在计算每个点的存水量，比如minHeap.poll\(\)出来的v1点，如果v1的没有visit的neighbor有v2，而且v2.height &lt; v1.height，那么v2的存水量不一定是\(v1.height - v2.height\)，因为在v1之前visit的v1.neighbor的height可能比v1高，假设在v1 visit前面visit的其neighbor为v3，那么v2的存水量可能为\(v3.height - v2.height\)。所以这里需要1个"bound"，一开始可以初始化 bound = 0，然后每次minHeap.poll\(\) 出来node后更新，bound = max\(bound, node.height\)，那么node的没有visit的neighbor的存水量就是 bound - 其高度  即可。

### 复杂度

O\(m \* n \* log\(m + n\)\)；m和n为2D matrix的2个维度。m \* n为总的点个数，log\(m+n\)为heap的1次操作的复杂度

### Code

```java
public class Solution {
    int[][] diff = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    class Node {
        int x;
        int y;
        int height;
        Node (int x, int y, int height) {
            this.x = x;
            this.y = y;
            this.height = height;
        }
    }

    public int trapRainWater(int[][] heightMap) {
        if(heightMap.length == 0) return 0;

        Queue<Node> minHeap = new PriorityQueue<Node>(new Comparator<Node>() {
            public int compare(Node n1, Node n2) {
                return n1.height - n2.height;
            }
        });
        int m = heightMap.length, n = heightMap[0].length;
        boolean[][] visited = new boolean[m][n];

        // 先把4周的点加入heap，并且标记visited，因为四周的点不会装水            
        for(int i = 0; i < m; i ++) {
            minHeap.add(new Node(i, 0, heightMap[i][0]));
            visited[i][0] = true;
            minHeap.add(new Node(i, n-1, heightMap[i][n-1]));
            visited[i][n-1] = true;
        }
        for(int i = 0; i < n; i ++) {
            minHeap.add(new Node(0, i, heightMap[0][i]));
            visited[0][i] = true;
            minHeap.add(new Node(m-1, i, heightMap[m-1][i]));
            visited[m-1][i] = true;
        }

        int rst = 0;
        int bound = 0; // 关键：需要1个bound记录目前visit到的装水的bound是多少
        while(!minHeap.isEmpty()) {
            Node node = minHeap.poll();
            bound = Math.max(bound, node.height);

            for(int i = 0; i < 4; i ++) {
                int x = node.x + diff[i][0], y = node.y + diff[i][1];
                if(x < m && x >=0 && y < n && y >= 0 && !visited[x][y]) {
                    visited[x][y] = true;
                    if(heightMap[x][y] < bound) rst += bound - heightMap[x][y];
                    minHeap.add(new Node(x, y, heightMap[x][y]));
                }
            }
        }
        return rst;
    }
}
```



