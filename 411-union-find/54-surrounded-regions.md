# 130. Surrounded Regions

> Given a 2D board containing`'X'`and `'O' `\(the **letter **O\), capture all regions surrounded by `'X'`.
>
> A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

### 思路

仔细观察题目和图形发现，从4周的“O” 开始向里面的neighbors做DFS，如果可以被visit到的“O”则不需要被set为“X”，反之需要被set 为"X"。

### Code

```java
public class Solution {
    public void solve(char[][] board) {
        if(board == null || board.length == 0) return;
        int n = board.length;
        int m = board[0].length;
        boolean[][] visited = new boolean[n][m];
        
        // mark "O" on boarder as visited to prepare for DFS visiting
        for(int i = 0 ; i < m; i++){  
            if(board[0][i] == 'O')  visited[0][i] = true;  
            if(board[n - 1][i] == 'O') visited[n - 1][i] = true;  
        }  
        for(int i = 0 ; i < n; i++){  
            if(board[i][0] == 'O') visited[i][0] = true;  
            if(board[i][m - 1] == 'O') visited[i][m - 1] = true;  
        }  
        
        // start DFS visiting from "O" on boarder
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                if(visited[i][j]) dfs(board, visited, i, j);
            }
        }
        
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                if(board[i][j] == 'O' && !visited[i][j]) board[i][j] = 'X';
            }
        }
    }
    
    public void dfs(char[][]board, boolean[][] visit, int i, int j) {
        // 注意Trick: 不能做 if(!visit[i][j]) return; 因为一开始从 boarder “0” visit时
        // visit[0][i] 是TRUE，但是我们需要继续DFS 往里visit
        if(board[i][j] == 'X') return;
        visit[i][j] = true;
        if(i > 0 && !visit[i - 1][j]) dfs(board, visit, i - 1, j);
        if(i < visit.length - 1 && !visit[i + 1][j]) dfs(board, visit, i + 1, j);
        if(j > 0 && !visit[i][j - 1]) dfs(board, visit, i, j - 1);
        if(j < visit[0].length - 1 && !visit[i][j + 1]) dfs(board, visit, i, j + 1);
    }
}
```



