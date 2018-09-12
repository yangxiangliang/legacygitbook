# 766. Toeplitz Matrix

> A matrix is _Toeplitz_  if every diagonal from top-left to bottom-right has the same element.
>
> Now given an`M x N`matrix, return `True`if and only if the matrix is _Toeplitz_.
>
> **Example 1:**
>
> ```
> Input: matrix = [[1,2,3,4],[5,1,2,3],[9,5,1,2]]
> Output: True
> Explanation:
> 1234
> 5123
> 9512
>
> In the above grid, the diagonals are "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]", and in each diagonal all elements are the same, so the answer is True.
> ```
>
> **Example 2:**
>
> ```
> Input: matrix = [[1,2],[2,2]]
> Output: False
> Explanation:
> The diagonal "[1, 2]" has different elements.
> ```

### Code

```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        int row = matrix.length;
        int col = matrix[0].length;
        
        for(int i = 0; i < col - 1; i ++) {
            int tmp = matrix[0][i];
            int x = 1, y = i + 1;
            while(x < row && y < col) {
                if(matrix[x++][y++] != tmp) return false;
            }
        }
        
        for(int i = 1; i < row - 1; i ++) {
            int tmp = matrix[i][0];
            int x = i + 1, y = 1;
            while(x < row && y < col) {
                if(matrix[x++][y++] != tmp) return false;
            }
        }
        return true;
    }
}
```



