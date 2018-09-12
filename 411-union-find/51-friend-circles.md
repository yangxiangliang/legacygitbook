# 547. Friend Circles

> There are **N **students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a **direct **friend of B, and B is a **direct **friend of C, then A is an **indirect **friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.
>
> Given a **N\*N **matrix **M **representing the friend relationship between students in the class. If M\[i\]\[j\] = 1, then the i th and j th students are **direct **friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.
>
> **Note:**
>
> 1. N is in range \[1,200\].
> 2. M\[i\]\[i\] = 1 for all students.
> 3. If M\[i\]\[j\] = 1, then M\[j\]\[i\] = 1.

### 思路

容易想到的union find，类似[**4.1**](/41.md)

还有第二种方法，就是直接DFS，因为M\[i\]\[j\] == 1的时候，就相当于 i 和 j之间有undirected edge。用一个boolean\[n\] visited 来记录是否visit过这个node\(一共有n个node，即n个student\)，对n个人都需要做DFS，但是在DFS过程中，有edge相连的都被标记visited了。一次DFS后 没有被visit的点就是另外一个friends circle的，此时用一个count记录 +1即可，然后继续对这个之间没有被visit的点做DFS即可。

### Code 1\)

```java
public class Solution {
    public int findCircleNum(int[][] M) {
        int n = M.length;
        int[] roots = new int[n];
        int count = n;
        for(int i = 0; i < n; i ++) roots[i] = i;
        for(int i = 0; i < n; i ++) {
            for(int j = i + 1; j < n; j ++) {
                if(M[i][j] == 1) {
                    int root1 = root(roots, i);
                    int root2 = root(roots, j);
                    // 注意只有 root1和root2不相同时才Union，不然skip
                    if(root1 != root2) {
                        // 注意Trick 易错：这里是union root1和root2，而不是union i 和 j
                        roots[root1] = root2;
                        count--;
                    }
                }
            }
        }
        return count;
    }

    private int root(int[] roots, int p) {
        while(p != roots[p]) {
            roots[p] = roots[roots[p]];
            p = roots[p];
        }
        return p;
    }
}
```

### Code 2\) DFS

```java
public class Solution {
    public int findCircleNum(int[][] M) {
        int count = 0;
        int n = M.length;
        boolean[] visited = new boolean[n];
        for(int i = 0; i < n; i ++) {
            if(visited[i] == false) {
                dfs(M, visited, i);
                count ++;
            }
        }
        return count;
    }

    private void dfs(int[][] M, boolean[] visited, int i) {
        visited[i] = true;
        for(int j = 0; j < M.length; j ++) {
            if(M[i][j] == 1 && visited[j] == false ) {
                dfs(M, visited, j);
            }
        }
    }
}
```



