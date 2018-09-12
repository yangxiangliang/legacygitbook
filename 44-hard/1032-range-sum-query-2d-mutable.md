# 308. Range Sum Query 2D - Mutable

> Given a 2D matrix, find the sum of the elements inside the rectangle defined by its upper left corner \(row1, col1\) and lower right corner \(row2, col2\).
>
> **Note:**
>
> 1. The matrix is only modifiable by the _update_ function.
> 2. You may assume the number of calls to _update_ and _sumRegion_ function is distributed evenly.
> 3. You may assume that _row1 &lt;= row2_ and _col1 &lt;= col2_

### 思路

**1\)** 由题意观察，如果我们用sum\(row, col\)表示从点\(row, col\)到\(0, 0\)的region的和\(inclusive\)。那么想到\(row1, col1\) 到 \(row2, col2\)的sumRegion值，等于sum\(row2, col2\) - sum\(row1 - 1, col2\) - sum\(row2, col1 - 1\) + sum\(row1 - 1, col1 - 1\)，当然注意row1, col1等于0 的情况。所以想到一开始求所有\(i, j\)的sum\(i, j \)的值，然后方便求得sumRegion\(row1, col1, row2, col2\)的值。update \(row, col\)的时候，只需要update所有包含点matrix\(row, col\)的sum的值即可。

#### **2\)** **思路1）中的sum\(row2, col2\) 相当于求一个array的prefix sum，只不过这里是2维数组的prefix sum。对于prefix sum问题，efficient的方法则是可以用Binary Indexed Tree，但是这里要把一般的一维的Binary Indexed Tree改造为2维的情况。**

### Code  1\)

```java
public class NumMatrix {
    int[][] matrix;
    int[][] count;
    int n, m;

    public NumMatrix(int[][] matrix) {
        if(matrix == null || matrix.length == 0) return;

        this.matrix = matrix;
        n = matrix.length;
        m = matrix[0].length;
        count = new int[n][m];
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                if(i == 0 && j == 0) count[0][0] = matrix[0][0];
                else if (i == 0) count[i][j] = count[i][j-1] + matrix[i][j];
                else if (j == 0) count[i][j] = count[i-1][j] + matrix[i][j];
                else count[i][j] = count[i-1][j] + count[i][j-1] + matrix[i][j] - count[i-1][j-1];
            }
        }
    }

    public void update(int row, int col, int val) {
        for(int i = row; i < n; i ++) {
            for(int j = col; j < m; j ++) {
                count[i][j] = count[i][j] - matrix[row][col] + val;
            }
        }
        matrix[row][col] = val;
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        int overlap = 0, upper = 0, left = 0;

        if(row1 > 0 && col1 > 0) overlap = count[row1-1][col1-1];
        if(row1 > 0) upper = count[row1-1][col2];
        if(col1 > 0) left = count[row2][col1-1];
        return count[row2][col2] - upper - left + overlap;
    }
}
```

### Code 2\)    使用2维的Binary Indexed Tree

Time Complexity: 因为Binary Indexed Tree的add和sum是O\(log\(n\)\)，这里为2维BIT，所以add和sum的complexity是              O\(log\(n\) \* log\(m\)\)，n和m是matrix的维度。

```java
public class NumMatrix {
    int[][] matrix;
    int[][] tree;
    int n;
    int m;

    public NumMatrix(int[][] matrix) {
        if(matrix == null || matrix.length == 0) return;

        n = matrix.length;
        m = matrix[0].length;
        tree = new int[n+1][m+1];
        this.matrix = new int[n][m]; // Do not assign it as this.matrix = matrix，只要initialize为0，然后update即可
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                update(i, j, matrix[i][j]);
            }
        }
    }

    public void update(int row, int col, int val) {
        int delta = val - matrix[row][col];
        matrix[row][col] = val;

        // 注意这里的index为(row+1)和(col+1)，因为Binary Indexed Tree的index对应原array的index+1        
        for(int i = row + 1; i <= n; i += i & (-i)) {
            for(int j = col + 1; j <= m; j += j & (-j)) {
                tree[i][j] += delta;
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return prefixSum(row2, col2) + prefixSum(row1-1, col1-1) - prefixSum(row1-1, col2) - prefixSum(row2, col1-1);
    }

    public int prefixSum(int row, int col) {
        int sum = 0;

        // 注意这里的index为(row+1)和(col+1)，因为Binary Indexed Tree的index对应原array的index+1
        for(int i = row + 1; i > 0; i -= i & (-i)) {
            for(int j = col + 1; j > 0; j -= j & (-j)) {
                sum += tree[i][j];
            }
        }
        return sum;
    }
}
```



