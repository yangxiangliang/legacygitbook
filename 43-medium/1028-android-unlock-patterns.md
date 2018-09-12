# 351. Android Unlock Patterns

> Given an Android **3x3 **key lock screen and two integers **m **and **n**, where 1 ≤ m ≤ n ≤ 9, count the total number of unlock patterns of the Android lock screen, which consist of minimum of **m **keys and maximum **n **keys.
>
> **Rules for a valid pattern: **
>
> 1. Each pattern must connect at least **m** keys and at most **n** keys.
> 2. All the keys must be distinct.
> 3. If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
> 4. The order of keys used matters.

### 思路

刚开始看这个题目有点没思路，感觉会用DFS解决，但是因为有可能可以连接非相邻的元素，有点不好处理。但是因为这里就是3x3的格子，那么可以罗列出可能被“跳过”的那些元素。比如连接1，3的时候，如果5已经被visit了，那么可以skip 5，直接连接1，3；反之则不行。而且要注意1和5，1和8这类的是可以直接连接的，因为中间没有“相隔”的元素。但是1到5，或则1到8这样的移动用坐标\(i, j\)不好表示，不是一般的“上，下，左，右”移动一步的表示。再考虑到这里元素一共只有9个，那么直接用1, 2......9 表示即可，boolean\[\] visited = new boolean\[10\] 记录被visit的情况，1对应visited\[1\] 即可。**注意Trick**：**因为1，3，7，9是对称的，从1开始的pattern数量 \* 4 便可以得到从1，3，7，9开始的pattern的总的数量。同理，2，4，6，8也是对称的。**

### Code

```java
public class Solution {
    public int numberOfPatterns(int m, int n) {
        boolean[] visited = new boolean[10];
        int[][] skip = new int[10][10];
        skip[1][3] = skip[3][1] = 2; // 表示1, 3 或则 3，1之间可能skip 2元素，取决于2是否已经被visit
        skip[4][6] = skip[6][4] = 5;
        skip[7][9] = skip[9][7] = 8;
        skip[1][7] = skip[7][1] = 4;
        skip[2][8] = skip[8][2] = 5;
        skip[3][9] = skip[9][3] = 6;
        skip[1][9] = skip[9][1] = skip[3][7] = skip[7][3] = 5;

        int count = 0;
        for(int i = m; i <= n; i ++) {
            // 因为1，3，7，9是对称的，从1开始后的pattern的数量 * 4即可，不用重新计算从3，7，9出发的pattern数量
            count += dfs(visited, skip, 1, i-1) * 4;
            count += dfs(visited, skip, 2, i-1) * 4;
            count += dfs(visited, skip, 5, i-1);
        }
        return count;
    }

    // cur表示当前visit的值，remains表示还需要visit多少个不同的key
    public int dfs(boolean[] visited, int[][] skip, int cur, int remains) {
        if(remains == 0) return 1;
        visited[cur] = true;
        int count = 0;
        for(int i = 1; i <= 9; i ++) {
            if(!visited[i] && (skip[cur][i] == 0 || visited[skip[cur][i]])) {
                count += dfs(visited, skip, i, remains - 1);
            }
        }
        // 注意visit完后，将其reset为false
        visited[cur] = false;
        return count;
    }
}
```



