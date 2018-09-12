# 750. Number Of Corner Rectangles

> Given a grid where each entry is only 0 or 1, find the number of corner rectangles.
>
> A _corner rectangle_ is 4 distinct 1s on the grid that form an axis-aligned rectangle. Note that only the corners need to have the value 1. Also, all four 1s used must be distinct.
>
> **Example 1:**
>
> ```
> Input: grid = 
> [[1, 0, 0, 1, 0],
>  [0, 0, 1, 0, 1],
>  [0, 0, 0, 1, 0],
>  [1, 0, 1, 0, 1]]
> Output: 1
> Explanation: There is only one corner rectangle, with corners grid[1][2], grid[1][4], grid[3][2], grid[3][4].
> ```
>
> **Example 2:**
>
> ```
> Input: grid = 
> [[1, 1, 1],
>  [1, 1, 1],
>  [1, 1, 1]]
> Output: 9
> Explanation: There are four 2x2 rectangles, four 2x3 and 3x2 rectangles, and one 3x3 rectangle.
> ```
>
> **Example 3:**
>
> ```
> Input: grid = 
> [[1, 1, 1, 1]]
> Output: 0
> Explanation: Rectangles must have four distinct corners.
> ```
>
> **Note:**
>
> 1. The number of rows and columns of grid will each be in the range \[1, 200\].
> 2. Each grid\[i\]\[j\] will be either 0 or 1.
> 3. The number of 1s in the grid will be at most 6000.

### 思路

1. 如果不想用brute force，先找规律怎么更有效地找Corner rectangle。首先 4 个Corner 1 必须在2个不同的row，那么可以在 row i, row j 的同一column的位置找是否\[i\]\[k\], \[j\]\[k\] 都是1，看 row i, row j 有多少个在同一column的位置元素都是1的情况，比如有 n 个，那么该 n 个边能构成的矩形的个数就是 n 中选择 2 的情况，即 n \* \(n-1\) / 2。如此这样遍历所以 row i, row j 的情况即可。

### 复杂度

Time： O\(row \* row \* col\)

### Code

```java
class Solution {
    
    int row;
    int col;
    public int countCornerRectangles(int[][] grid) {
        if(grid.length < 2) return 0;
        row = grid.length;
        col = grid[0].length;     
        int rst = 0;
        for(int i = 0; i < row - 1; i ++) {
            for(int j = i + 1; j < row; j ++) {
                
                int count = 0;
                for(int k = 0; k < col; k ++) {
                    if(grid[i][k] == 1 && grid[j][k] == 1) count ++;
                }
                rst += count * (count - 1) / 2;
            }
        }
        return rst;
    }
}
```



