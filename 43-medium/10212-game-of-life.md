# 289. Game of Life

> According to the [Wikipedia's article](https://en.wikipedia.org/wiki/Conway's_Game_of_Life) : "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

> Given a board with m by n cells, each cell has an initial state live \(1\) or dead \(0\). Each cell interacts with its [eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood) \(horizontal, vertical, diagonal\) using the following four rules \(taken from the above Wikipedia article\):
>
> 1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
> 2. Any live cell with two or three live neighbors lives on to the next generation.
> 3. Any live cell with more than three live neighbors dies, as if by over-population.
> 4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
>
> Write a function to compute the next state \(after one update\) of the board given its current state.
>
> **Follow up:**
>
> 1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
> 2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

### 思路

这里需要in-place，而且board上面所有的元素需要update at the same time。所以此时我们不能立即update current 元素，不然一会visit到其附近的neighbors时候，可能会利用其update后的值。

因为只有1-&gt;0和1-&gt;0 两种变化形式，这里想到的方法是：当1-&gt;0的时候，将current value暂时设置为2；当0-&gt;1的时候，将current value暂时设置为-1。这样在我们找neighbor中的时候，如果value &lt; -1，我们都可以当做是dead，如果value &gt; 0，我们都可以当做是live的，不会收影响。最后我们把board上面的2 设为0，-1设置为1 即可。

### Code

```java
public class Solution {
    int[] diffx = new int[]{0, 0, -1, -1, -1, 1, 1, 1};
    int[] diffy = new int[]{-1, 1, -1, 0, 1, -1, 0, 1};

    public void gameOfLife(int[][] board) {
        int m = board.length;
        int n = board[0].length;
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                int count = getLiveNeighborsNum(board, i, j, m, n);
                if(board[i][j] == 0 && count == 3) board[i][j] = -1;
                if(board[i][j] == 1 && (count < 2 || count > 3)) board[i][j] = 2;
            }
        }

        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(board[i][j] == 2) board[i][j] = 0;
                if(board[i][j] == -1) board[i][j] = 1;
            }
        }
    }

    public int getLiveNeighborsNum(int[][] board, int i, int j, int m, int n) {
        int count = 0;
        for(int k = 0; k < 8; k ++) {
            int x = diffx[k] + i;
            int y = diffy[k] + j;
            if(x >= 0 && x < m && y >= 0 && y < n && board[x][y] > 0) count ++;
        }
        return count;
    }
}
```

**另外，在网上看见一个大体思想类似，但是**[**更加巧妙的方法**](https://discuss.leetcode.com/topic/29054/easiest-java-solution-with-explanation)**。用了2个bit来表示不同的状态，挺巧妙。**

