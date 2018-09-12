# 348. Design Tic-Tac-Toe

> Design a Tic-tac-toe game that is played between two players on a _n x n_ grid.
>
> You may assume the following rules:
>
> 1. A move is guaranteed to be valid and is placed on an empty block.
> 2. Once a winning condition is reached, no more moves is allowed.
> 3. A player who succeeds in placing _n_ of their marks in a horizontal, vertical, or diagonal row wins the game.
>
> **Example:**
>
> ```
> Given n = 3, assume that player 1 is "X" and player 2 is "O" in the board.
>
> TicTacToe toe = new TicTacToe(3);
>
> toe.move(0, 0, 1); -> Returns 0 (no one wins)
> |X| | |
> | | | |    // Player 1 makes a move at (0, 0).
> | | | |
>
> toe.move(0, 2, 2); -> Returns 0 (no one wins)
> |X| |O|
> | | | |    // Player 2 makes a move at (0, 2).
> | | | |
>
> toe.move(2, 2, 1); -> Returns 0 (no one wins)
> |X| |O|
> | | | |    // Player 1 makes a move at (2, 2).
> | | |X|
>
> toe.move(1, 1, 2); -> Returns 0 (no one wins)
> |X| |O|
> | |O| |    // Player 2 makes a move at (1, 1).
> | | |X|
>
> toe.move(2, 0, 1); -> Returns 0 (no one wins)
> |X| |O|
> | |O| |    // Player 1 makes a move at (2, 0).
> |X| |X|
>
> toe.move(1, 0, 2); -> Returns 0 (no one wins)
> |X| |O|
> |O|O| |    // Player 2 makes a move at (1, 0).
> |X| |X|
>
> toe.move(2, 1, 1); -> Returns 1 (player 1 wins)
> |X| |O|
> |O|O| |    // Player 1 makes a move at (2, 1).
> |X|X|X|
> ```
>
> **Follow up:**
>
> Could you do better than O\(n^2\) per `move()`operation?

### 思路

Brute force的方法就是每次move后数该点的row, column 或则diagonal方向上该player的个数，如果是n，即是满的则return 该player即可。显然这样太麻烦了，应该用个东西记录以前在该row, column, diagonal 上该player已经放置的个数，然后此时只需要+1，如果+1后的结果等于n，则说明该player胜利。

### Code

```java
public class TicTacToe {
    int n;
    int[][] colCount;
    int[][] rowCount;
    int[] leftDiag;
    int[] rightDiag;

    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        this.n = n;
        // colCount[0][i]表示player1在第i个column上的个数，colCount[1][i]表示player2在第i个column上的个数
        colCount = new int[2][n];
        // rowCount[0][i]表示player1在第i个row上的个数，rowCount[1][i]表示player2在第i个row上的个数
        rowCount = new int[2][n];
        // 记录从左上到右下diagonal的情况，leftDiag[0]表示player1的个数，leftDiag[1]表示player2的个数
        leftDiag = new int[2];
        // 记录从右上到左下diagonal的情况，rightDiag[0]表示player1的个数，rightDiag[1]表示player2的个数
        rightDiag = new int[2];
    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) {
        if(++colCount[player - 1][col] == n) return player;
        if(++rowCount[player - 1][row] == n) return player;
        if(row == col) {
            if (++leftDiag[player - 1] == n) return player;
        }
        if(row == n - 1 - col) {
            if(++rightDiag[player - 1] ==n) return player;
        }
        return 0;
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```



