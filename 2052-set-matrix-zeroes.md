# 73. Set Matrix Zeroes

> Given a _m_ x _n_ matrix, if an element is 0, set its entire row and column to 0. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm)
>
> **Example 1:**
>
> ```
> Input: 
> [
>   [1,1,1],
>   [1,0,1],
>   [1,1,1]
> ]
>
> Output:
> [
>   [1,0,1],
>   [0,0,0],
>   [1,0,1]
> ]
> ```
>
> **Example 2:**
>
> ```
> Input: 
> [
>   [0,1,2,0],
>   [3,4,5,2],
>   [1,3,1,5]
> ]
>
> Output: 
> [
>   [0,0,0,0],
>   [0,4,5,0],
>   [0,3,1,0]
> ]
> ```
>
> **Follow up：**Could you devise a constant space solution?

### 思路

1. 因为这里不能用额外空间。那么不能额外开个matrix来存是否visit过。只能利用input matrix 来缓存中间的结果。
2. 可以利用 first row，first column来存。如果发现\[i, j\] == 0，那么可以把其对应的first row, first column的中的元素也set为0。最后扫一遍 first row, first column的元素，如果\[i, 0\] == 0，则表示 i row 必须全部为0；如果 \[0, j\] == 0，则便是 j column 必须全部为0。
3. **注意** 这里需要注意怎么来表示 first row, first column 是否该全部为0。因为原来input matrix中\[i, 0\]为0的时候，表示除了i row，那么first column需要全部也set 0。那么就用2个variable 来表示是否需要把 first row, first column 也全部set 0 即可。

### Code

```java
public class Solution {
    public void setZeroes(int[][] matrix) {
        boolean row_zero=false;
        boolean col_zero=false;
        int row=matrix.length;
        int col=matrix[0].length;
        for(int i=0;i<row;i++){
            if(matrix[i][0]==0){
                col_zero=true;
                break;
            }
        }
        for(int i=0;i<col;i++){
            if(matrix[0][i]==0){
                row_zero=true;
                break;
            }
        }
        for(int i=1;i<row;i++){
            for(int j=1;j<col;j++){
                if(matrix[i][j]==0){
                    matrix[i][0]=0;
                    matrix[0][j]=0;
                }
            }
        }
        for(int i=1;i<row;i++){
            for(int j=1;j<col;j++){
                if(matrix[i][0]==0||matrix[0][j]==0) 
                   matrix[i][j]=0;
            }
        }
        if(row_zero){
            for(int i=0;i<col;i++)
               matrix[0][i]=0;
        }
        if(col_zero){
            for(int i=0;i<row;i++)
               matrix[i][0]=0;
        }
    }
}
```



