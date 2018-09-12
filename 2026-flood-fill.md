# 733. Flood Fill

> An`image`is represented by a 2-D array of integers, each integer representing the pixel value of the image \(from 0 to 65535\).
>
> Given a coordinate`(sr, sc)`representing the starting pixel \(row and column\) of the flood fill, and a pixel value`newColor`, "flood fill" the image.
>
> To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels \(also with the same color as the starting pixel\), and so on. Replace the color of all of the aforementioned pixels with the newColor.
>
> At the end, return the modified image.
>
> **Example 1:**
>
> ```
> Input: 
> image = [[1,1,1],[1,1,0],[1,0,1]]
> sr = 1, sc = 1, newColor = 2
> Output: [[2,2,2],[2,2,0],[2,0,1]]
> Explanation: 
> From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected 
> by a path of the same color as the starting pixel are colored with the new color.
> Note the bottom corner is not colored 2, because it is not 4-directionally connected
> to the starting pixel.
> ```
>
> **Note:**
>
> * The length of`image`and`image[0]`will be in the range`[1, 50]`.
>
> * The given starting pixel will satisfy`0 <= sr <image.length`and`0 <= sc <image[0].length`.
>
> * The value of each color in`image[i][j]`and`newColor`will be an integer in`[0, 65535]`.

### 思路

根据题目意思“演绎”color上色的过程，就是从src点开始一个上，下，左，右的DFS过程。如果visit的点与src颜色相同，那么上成新的颜色，继续上下左右DFS。同时用visited\[\]\[\] map记录哪些点visit过了，避免重复visit。

### Code

```java
class Solution {
    int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        boolean[][] visited = new boolean[image.length][image[0].length];
        fill(image, sr, sc, newColor, image[sr][sc], visited);
        return image;
    }
    
    private void fill(int[][] image, int i, int j, int newColor, int originColor, boolean[][] visited) {
        if(i >= image.length || i < 0 || j < 0 || j >= image[0].length) return;
        if(visited[i][j]) return;
        visited[i][j] = true;
        if(image[i][j] != originColor) return;
        image[i][j] = newColor;
        for(int[] dir : directions) {
            fill(image, i+dir[0], j+dir[1], newColor, originColor, visited);
        }
    }
}
```



