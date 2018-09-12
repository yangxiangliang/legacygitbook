# 37. Sudoku Solver

> Write a program to solve a Sudoku puzzle by filling the empty cells.
>
> Empty cells are indicated by the character`'.'`.
>
> You may assume that there will be only one unique solution.

### 思路

1. 题目和 [20.2 Valid Sudoku](/11-valid-sudoku.md)  有类似的，但是不光是判断sudoku是不是valid，而是要解决该Sudoku。这里对于empty的格子，并不知道该填1到9哪一个，只有一个一个试，然后看哪个路径能走得通。所以又是典型的backtracking的感觉。
2. 这个有个optimization的地方，就是检查改变空格后的board是不是valid的时候，不用检查整个board，只用检查改变的格子所对应的row, column 和 center bucket即可。

### Code

```java
class Solution {
    public void solveSudoku(char[][] board) {
        if(board.length == 0) return;
        solve(board);
    }

    private boolean solve(char[][] board) {
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board[0].length; j ++) {
                if(board[i][j] == '.') {
                    for(char c = '1'; c <= '9'; c++) {
                        board[i][j] = c;
                        if(isValid(board, i, j) && solve(board)) return true;
                    }
                    board[i][j] = '.';
                    return false;
                }
            }
        }
        // 不要 return false, 这里表示没有‘.’在board中，表示已经solve board了，所以return true
        return true;
    }

    private boolean isValid(char[][] board, int row, int col) {
        Set<Character> rowSet = new HashSet();
        Set<Character> colSet = new HashSet();
        Set<Character> centerSet = new HashSet();

        for(int i = 0; i < board[0].length; i ++) {
            if(board[row][i] == '.') continue;
            if(rowSet.contains(board[row][i])) return false;
            rowSet.add(board[row][i]);
        }

        for(int i = 0; i < board.length; i ++) {
            if(board[i][col] == '.') continue;
            if(colSet.contains(board[i][col])) return false;
            colSet.add(board[i][col]);
        }

        int startRow = row / 3 * 3;
        int startCol = col / 3 * 3;
        for(int i = startRow; i < startRow + 3; i ++) {
            for(int j = startCol; j < startCol + 3; j ++) {
                if(board[i][j] == '.') continue;
                if(centerSet.contains(board[i][j])) return false;
                centerSet.add(board[i][j]);
            }
        }
        return true;
    }
}
```



