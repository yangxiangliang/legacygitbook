# 361. Bomb Enemy

> Given a 2D grid, each cell is either a wall `'W'`, an enemy`'E' `or empty `'0' `\(the number zero\), return the maximum enemies you can kill using one bomb.
>
> The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.
>
> Note that you can only put the bomb at an empty cell.

### 思路  1\)

一般的DFS，用int\[\]\[\] count记录每个empty的spot能炸到的enemy的个数。从“E”的地方为原点，分别向上、下、左、右 做DFS知道遇到border 或则“W”，没遇见一个“0” 点\(i ,  j\)，便做count\[i\]\[j\] ++，并且update result 最大值 max即可。

**该方法速度特别慢，因为很多点都会被重复visit。**

```java
public class Solution {
    int max = 0;
    
    public int maxKilledEnemies(char[][] grid) {
        if(grid == null || grid.length == 0) return 0;
        int n = grid.length;
        int m = grid[0].length;
        int[][] count = new int[n][m];
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                if(grid[i][j] == 'E') helperDFS(grid, count, i, j);
            }
        }
        return max;
    }
    
    public void helperDFS(char[][] grid, int[][] count, int i, int j) {
        int a = i;
        int b = j;
        while(a >= 0 && grid[a][b] != 'W') {
            if(grid[a][b] == '0') count[a][b] ++;
            max = Math.max(max, count[a][b]);
            a --;
        }
        a = i;
        while(a < grid.length && grid[a][b] != 'W') {
            if(grid[a][b] == '0') count[a][b] ++;
            max = Math.max(max, count[a][b]);
            a ++;
        }
        a = i;
        while(b >= 0 && grid[a][b] != 'W') {
            if(grid[a][b] == '0') count[a][b] ++;
            max = Math.max(max, count[a][b]);
            b --;
        }
        b = j;
        while(b < grid[0].length && grid[a][b] != 'W') {
            if(grid[a][b] == '0') count[a][b] ++;
            max = Math.max(max, count[a][b]);
            b ++;
        }
    }
}
```

### 思路  2） Dynamic Programming

同样需要2个for loops，但是在横向遍历的时候，我们只需用一个variable row记录横向可以炸到的enemy的个数即可，后面横向的点都可以reuse这个variable，但是如果遇见"W"，则之前记录的row可以炸到的enemy个数需要重新开始计算。同理，纵向，需要一个int array记录每个纵向可以炸到的enemy的个数，同样也可以reuse这个array记录。 

**Time Complexity: O\(n \* m\)，因为每个点都只会被visit一次，不会被重复visit**

```java
public class Solution {
    public int maxKilledEnemies(char[][] grid) {
        if(grid == null || grid.length == 0) return 0;
        int n = grid.length;
        int m = grid[0].length;
        int max = 0;
        int row = 0; // 表示横向可以炸到的enemy的数量
        int[] cols = new int[m]; // 用array 记录纵向可以炸到的enemy的数量
        
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                if(grid[i][j] == 'W') continue;
                if(j == 0 || grid[i][j-1] == 'W') row = killRowEnemy(grid, i , j);
                if(i == 0 || grid[i-1][j] == 'W') cols[j] = killColEnemy(grid, i, j);
                if(grid[i][j] == '0') max = Math.max(max, row + cols[j]);
            }
        }
        return max;
    }
    
    public int killRowEnemy(char[][] grid, int i, int j) {
        int num = 0;
        while(j < grid[0].length && grid[i][j] != 'W') {
            if(grid[i][j] == 'E') num ++;
            j ++;
        }
        return num;
    }
    
    public int killColEnemy(char[][] grid, int i, int j) {
        int num = 0;
        while(i < grid.length && grid[i][j] != 'W') {
            if(grid[i][j] == 'E') num ++;
            i ++;
        }
        return num;
    }
}
```







