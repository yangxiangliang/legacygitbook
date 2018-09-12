# 311. Sparse Matrix Multiplication

> Given two [sparse matrices](https://en.wikipedia.org/wiki/Sparse_matrix) **A **and **B**, return the result of **AB**.
>
> You may assume that **A**'s column number is equal to **B**'s row number.
>
> **Example:**
>
> ```
> Input:
>
> A = [
>   [ 1, 0, 0],
>   [-1, 0, 3]
> ]
>
> B = [
>   [ 7, 0, 0 ],
>   [ 0, 0, 0 ],
>   [ 0, 0, 1 ]
> ]
>
> Output:
>
>      |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
> AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
>                   | 0 0 1 |
> ```

### Code

```java
class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        int a_row = A.length;
        int a_col = A[0].length;
        int b_col = B[0].length;
        
        int[][] rst = new int[a_row][b_col];
        for(int i = 0; i < a_row; i ++) {
            for(int k = 0; k < a_col; k ++) {
                if(A[i][k] != 0) {
                    for(int j = 0; j < b_col; j ++) {
                        rst[i][j] += A[i][k] * B[k][j];
                    }
                }
            }
        }
        return rst;
    }
}

/* 
  注意
   for (int i = 0; i < a_row; i++) {
        for (int j = 0; j < b_col; j++) {
            for (int k = 0; k < a_col; k++) {
                rst[i][j] += A[i][k]*B[k][j];
            }
        }
    } 
    是最好理解的计算过程，因为是sparse matrix,很多A[i][k]都可能是0，所以交换第2个，第3个for loop, 如果A[i][k] == 0
    我们可以不进入第3个for loop计算。
*/
```



