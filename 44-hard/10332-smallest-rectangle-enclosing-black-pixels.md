# 302. Smallest Rectangle Enclosing Black Pixels

> An image is represented by a binary matrix with `0`as a white pixel and`1`as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location`(x, y)`of one of the black pixels, return the area of the smallest \(axis-aligned\) rectangle that encloses all black pixels.
>
> For example, given the following image:
>
> ```
> [
>   "0010",
>   "0110",
>   "0100"
> ]
> ```
>
> and `x = 0`, `y = 2`,  Return`6`.

### 思路

一开始想到的方法就是从\(x, y\)点开始DFS，因为black pixels都是连在一起的，所以很容易找出left, right, up, down 4个boundaries，然后这4个boundaries所组成的矩形的面积就是smallest rectangle的面积值。

### 复杂度

O\(m \* n\)

### Code

```java
class Solution {
    // black pixels的4个boundaries的值
    int left, right, up, down;

    public int minArea(char[][] image, int x, int y) {
        int m = image.length;
        int n = image[0].length;

        left = n-1;
        right = 0;
        up = m-1;
        down = 0;
        boolean[][] visited = new boolean[m][n];
        dfs(image, visited, x, y, m, n);
        return (right - left + 1) * (down - up + 1);
    }

    public void dfs(char[][] image, boolean[][] visited, int x, int y, int m, int n) {
        if(x < 0 || x >= m || y < 0 || y >= n || visited[x][y] || image[x][y] == '0') return;
        visited[x][y] = true;

        up = Math.min(x, up);
        down = Math.max(x, down);
        left = Math.min(y, left);
        right = Math.max(y, right);

        dfs(image, visited, x-1, y, m, n);
        dfs(image, visited, x+1, y, m, n);
        dfs(image, visited, x, y-1, m, n);
        dfs(image, visited, x, y+1, m, n);
    }
}
```





