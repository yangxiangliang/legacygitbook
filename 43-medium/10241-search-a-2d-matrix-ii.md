# 240. Search a 2D Matrix II

> Write an efficient algorithm that searches for a value in an \_m x n  \_matrix. This matrix has the following properties:
>
> * Integers in each row are sorted in ascending from left to right.
> * Integers in each column are sorted in ascending from top to bottom.
>
> For example,
>
> Consider the following matrix:
>
> ```
> [
>   [1,   4,  7, 11, 15],
>   [2,   5,  8, 12, 19],
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],
>   [18, 21, 23, 26, 30]
> ]
> ```
>
> Given **target **=`5`, return `true`.
>
> Given **target **=`20`, return `false`.

### 思路

一开始看见matrix，因为row和col都是sorted的，最容易想到binary search，但是看了下2D的感觉binary search也不是很容易。仔细观察，因为matrix中left to right, top to bottom是ascending的，所以从右上角开始，恰好是在ascending的中间，因为其左边的都小于它，下面的都大于它。那么可以target和右上角开始比，如果target更大，说明不可能在row = 0中，可以舍弃 row = 0；如果target更小，说明不在其相同的column中，可以舍弃它所在的column。如此下去看能不能找到target。

### 复杂度

O\(M + N\), M = matrix.length, N = matrix\[0\].length

### Code

```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix.length == 0) return false;

        int m = matrix.length;
        int n = matrix[0].length;
        int row = 0, col = n - 1;
        while(row < m && col >= 0) {
            if(matrix[row][col] == target) return true;
            else if(target < matrix[row][col]) col --;
            else row ++;
        }
        return false;
    }
}
```



