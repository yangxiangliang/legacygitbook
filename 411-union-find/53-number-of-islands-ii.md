# 305. Number of Islands II

> A 2d grid map of `m`rows and `n`columns is initially filled with water. We may perform an addLand operation which turns the water at position \(row, col\) into a land. Given a list of positions to operate, **count the number of islands after each addLand operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
>
> **Challenge:**
>
> Can you do it in time complexity O\(k log mn\), where k is the length of the `positions`?

### 思路

也是用Union Find，当每新加入一个1时，遍历其上下左右4个neighbors，有没有1。默认新加入的1可以加一个island，假设此值为count = 1, 如果neighbor中有1，看其root一不一样，如果不一样，则可以union，那么count -1，因为union后是不加入新island的；再看另外3个neighbor有没有1，如果有，而且root还不一样，那么还可以union，并且count -1，说明加入这个新的1后islands的总数还要 减 1，如此遍历其4个neighbors。同样用int\[\] roots 来记录所有的roots点，那么grid\[i\]\[j\] 对应roots中的index可以为 i \* n + j，n是grid的column数目。

**Improvements：**如果加上 **Path Compression find** 和 **Union by rank** ， 可以达到题目中要求的Time Complexity

### Code

```java
public class Solution {
    int[] roots;
    int[] size;

    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        roots = new int[m*n];
        size = new int[m*n];
        int[][] grid = new int[m][n];
        List<Integer> rst = new ArrayList();

        for(int[] pair : positions) {
            int i = pair[0];
            int j = pair[1];
            grid[i][j] = 1;

            int newIslands = 1; // 新加入的islands的个数，初始化为1
            int index = getIndex(i, j, n);
            roots[index] = index; // 初始root值为 i * n + j

            if(i > 0 && grid[i-1][j] == 1) newIslands = union(index, i-1, j, n, newIslands);
            if(j > 0 && grid[i][j-1] == 1) newIslands = union(index, i, j-1, n, newIslands);
            if(i < m-1 && grid[i+1][j] == 1) newIslands = union(index, i+1, j, n, newIslands);
            if(j < n-1 && grid[i][j+1] == 1) newIslands = union(index, i, j+1, n, newIslands);

            if(rst.size() == 0) rst.add(newIslands);
            else {
                int last = rst.get(rst.size() - 1);
                rst.add(last + newIslands);
            }
        }
        return rst;
    }

    public int union(int index2, int i, int j, int n, int newIslands) {
        int index1 = getIndex(i, j, n);
        int root1 = root(roots, index1);
        int root2 = root(roots, index2);
        if(root1 != root2) {
            // union by weight
            if(size[root1] < size[root2]) {
                roots[root1] = root2;
                size[root2] += size[root1];
            }else {
                roots[root2] = root1;
                size[root1] += size[root2];
            }
            return newIslands - 1;
        }
        return newIslands;
    }

    public int getIndex(int i, int j, int n) {
        return i * n + j;
    }

    public int root(int[] roots, int p) {
        while(p != roots[p]) {
            roots[p] = roots[roots[p]]; // path compression
            p = roots[p];
        }
        return p;
    }
}
```



