# 221. Maximal Square

> Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.
>
> **Example:**
>
> ```
> Input: 
>
> 1 0 1 0 0
> 1 0 1 1 1
> 1 1 1 1 1
> 1 0 0 1 0
>
> Output: 4
> ```

### 思路

DP思维，\[i\]\[j\] 处为1的话，其往左，或则往上能连成1的长度，其实等于\[i-1\]\[j-1\], \[i-1\]\[j\], \[i\]\[j-1\] 处往左或则往上能连成1的长度的

最小值 + 1。

### Code

```java
public class Solution {
    public int maximalSquare(char[][] matrix) {
        int row = matrix.length;
        if(row == 0) return 0;
        int col = matrix[0].length;
        
        // 把dp的长度和宽度设置为(row + 1), (col + 1)。方便后面写code，因为这样操作matrix[0][j]的时候，
        // 不用特别handle第一个index变为negative的情况
        int[][] dp = new int[row + 1][col + 1];
        int rst = 0;
        for(int i = 1; i <= row; ++i) {
            for(int j = 1; j <= col; ++j) {
                if(matrix[i - 1][j - 1] == '1') {
                    dp[i][j] = Math.min(Math.min(dp[i-1][j-1], dp[i-1][j]), dp[i][j-1]) + 1;
                    rst = Math.max(rst, dp[i][j]);
                }
            }
        }
        return rst * rst;
    }
}
```



