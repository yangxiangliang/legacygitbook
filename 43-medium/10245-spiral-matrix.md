# 54. Spiral Matrix

> Given a matrix of _m x n_ elements \(_m rows, n columns_\), return all elements of the matrix in spiral order.
>
> For example,
>
> Given the following matrix:
>
> ```
> [
>  [ 1, 2, 3 ],
>  [ 4, 5, 6 ],
>  [ 7, 8, 9 ]
> ]
> ```
>
> You should return `[1,2,3,6,9,8,7,4,5]`.

### 思路

思路就是用4个pointer，一个指向most upper row, 一个指向most lower row，一个指向 most right column, 一个指向 most left column。然后分4个步骤，先在most upper row从左到右遍历，然后upper row pointer 减 1；然后most right column 从上到下遍历，right column pointer 减 1; 然后most lower row 从右到左遍历，lower row 加 1; 然后most left column 从下到上遍历，most left column 加 1。如此继续，直到这几个pointer 交叉。

### Code

```java
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> rst = new ArrayList();
        if(matrix.length == 0) return rst;

        int upperRow = 0, lowerRow = matrix.length - 1, leftCol = 0, rightCol = matrix[0].length - 1;
        while(upperRow <= lowerRow && leftCol <= rightCol) {
            // process most upper row
            for(int i = leftCol; i <= rightCol; i ++) rst.add(matrix[upperRow][i]);
            upperRow ++;

            // process right most column
            for(int i = upperRow; i <= lowerRow; i ++) rst.add(matrix[i][rightCol]);
            rightCol --;

            // Note: 进入while循环的时候，可能lowerRow和upperRow相等，在上面upperRow ++后，其比lowerRow大
            // 相同于已经遍历过了lowerRow了，这里不用遍历lowerRow
            if(lowerRow >= upperRow) {
                // process most lower row
                for(int i = rightCol; i >= leftCol; i --) rst.add(matrix[lowerRow][i]);
                lowerRow --;
            }

            // Note: check rightCol 和 leftCol的原理同上
            if(rightCol >= leftCol) {
                // process left most column
                for(int i = lowerRow; i >= upperRow; i --) rst.add(matrix[i][leftCol]);
                leftCol ++;
            }
        }
        return rst;
    }
}
```



