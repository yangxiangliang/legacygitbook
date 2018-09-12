# 36. Valid Sudoku

> Determine if a Sudoku is valid, according to:[Sudoku Puzzles - The Rules](http://sudoku.com.au/TheRules.aspx).
>
> The Sudoku board could be partially filled, where empty cells are filled with the character`'.'`.
>
> **Note:**
>
> A valid Sudoku board \(partially filled\) is not necessarily solvable. Only the filled cells need to be validated.

### 思路

思路很容易，就是用一些HashSet来记录每一个row，每一个column，每一个cube中是否有1-9的元素重复情况。如果有，则return false。

### Code

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        for(int i = 0; i < board.length; i ++) {
            // 注意每次需要clear HashSet
            Set<Character> rowSet = new HashSet();
            Set<Character> colSet = new HashSet();
            Set<Character> centerSet = new HashSet();

            for(int j = 0; j < board[0].length; j ++) {
                if(board[i][j] != '.' && colSet.contains(board[i][j])) return false;
                colSet.add(board[i][j]);
                
                // 这里的i, j是相对的，所以i, j换位置后，可以适用于row set和column set，可以简化code
                if(board[j][i] != '.' && rowSet.contains(board[j][i])) return false;
                rowSet.add(board[j][i]);
                
                // Note: 注意用(i, j) 来表示cube中元素的坐标方式
                int x = i / 3 * 3 + j / 3;
                int y = i % 3 * 3 + j % 3;
                if(board[x][y] != '.' && centerSet.contains(board[x][y])) return false;
                centerSet.add(board[x][y]);
            }
        }
        return true;
    }
}
```



