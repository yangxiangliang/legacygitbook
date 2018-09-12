# 764. Largest Plus Sign

> In a 2D`grid`from \(0, 0\) to \(N-1, N-1\), every cell contains a`1`, except those cells in the given list`mines`which are`0`. What is the largest axis-aligned plus sign of`1`s contained in the grid? Return the order of the plus sign. If there is none, return 0.
>
> An "axis-aligned plus sign of`1`sof order **k**" has some center`grid[x][y] = 1`along with 4 arms of length`k-1`going up, down, left, and right, and made of`1`s. This is demonstrated in the diagrams below. Note that there could be`0`s or`1`s beyond the arms of the plus sign, only the relevant area of the plus sign is checked for 1s.
>
> **Examples of Axis-Aligned Plus Signs of Order k:**
>
> ```
> Order 1:
> 000
> 010
> 000
>
> Order 2:
> 00000
> 00100
> 01110
> 00100
> 00000
>
> Order 3:
> 0000000
> 0001000
> 0001000
> 0111110
> 0001000
> 0001000
> 0000000
> ```
>
> **Example 1:**
>
> ```
> Input: N = 5, mines = [[4, 2]]
> Output: 2
> Explanation:
> 11111
> 11111
> 11111
> 11111
> 11011
> In the above grid, the largest plus sign can only be order 2.  One of them is marked in bold.
> ```
>
> **Example 2:**
>
> ```
> Input: N = 2, mines = []
> Output: 1
> Explanation:
> There is no plus sign of order 2, but there is of order 1.
> ```
>
> **Example 3:**
>
> ```
> Input: N = 1, mines = [[0, 0]]
> Output: 0
> Explanation:
> There is no plus sign, so return 0.
> ```
>
> **Note:**
>
> 1. N will be an integer in the range \[1, 500\]
> 2. mines will have length at most 5000.
> 3. mines\[i\] will be length 2 and consist of integers in the range \[0, N-1\].

### 思路

1. 首先看怎么计算largest plus sign order，对于grid中点\[i\]\[j\]，如果gird\[i\]\[j\] == 0，其无法形成 plus sign，所以order肯定是0。如果\[i\]\[j\] == 1，那么需要看起4个方向 “上，下，左，右” 形成连续的1的长度是多少，取该4个长度的最小值便是以\[i\]\[j\] 为中心能形成的plus sign 的order。
2. 那么这里容易想到用4个不同的grid，分别来记录每个点的“上，下，左，右”能形成的连续的1的长度。方便最后计算每个点为中心的plus sign的order。计算该4个grid的时候，明显 \[i\]\[j\] “向上” 能形成的连续的1的长度，和\[i-1\]\[j\] 向上能形成的连续的1的长度相关，所以可以用DP的思维来计算。

### Code

```java
class Solution {
    public int orderOfLargestPlusSign(int N, int[][] mines) {
        int[][] grid = new int[N][N];
        for(int i = 0; i < N; i ++) {
            Arrays.fill(grid[i], 1);
        }
        for(int[] pair : mines) {
            grid[pair[0]][pair[1]] = 0;
        }

        int[][] up = new int[N][N];
        int[][] down = new int[N][N];
        int[][] left = new int[N][N];
        int[][] right = new int[N][N];

        for(int i = 0; i < N; i ++) {
            for(int j = 0; j < N; j ++) {
                if(grid[i][j] == 0) {
                    up[i][j] = 0;
                    left[i][j] = 0;
                } else {
                    up[i][j] = 1 + (i > 0 ? up[i-1][j] : 0);
                    left[i][j] = 1 + (j > 0 ? left[i][j-1] : 0);
                }

                // 因为2个for循环的顺序是“从上到下，从左到右”，但是计算 down, right grid的时候
                // 需要“从下到上，从右到左”计算，所以计算down, right grid的时候index需要稍微改变一下
                if(grid[N - 1- i][j] == 0) {
                    down[N - 1 - i][j] = 0;
                } else {
                    down[N - 1 - i][j] = 1 + (i > 0 ? down[N - i][j] : 0);
                }

                if(grid[i][N - 1 - j] == 0) {
                    right[i][N - 1 - j] = 0;
                } else {
                    right[i][N - 1- j] = 1 + (j > 0 ? right[i][N - j]: 0);
                }
            }
        }

        int rst = 0;
        for(int i = 0; i < N; i ++) {
            for(int j = 0; j < N; j ++) {
                int curLength = Math.min(Math.min(Math.min(up[i][j], down[i][j]), left[i][j]), right[i][j]);
                rst = Math.max(curLength, rst);
            }
        }
        return rst;
    }
}
```



