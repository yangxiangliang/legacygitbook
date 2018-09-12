# 276. Paint Fence

> There is a fence with n posts, each post can be painted with one of the k colors.
>
> You have to paint all the posts such that no more than two adjacent fence posts have the same color.
>
> Return the total number of ways you can paint the fence.
>
> **Note:**  
> n and k are non-negative integers.

### 思路

需要用DP的思想，第i个post的颜色决定于之前2个post 颜色的情况。由于i和\(i-1\)的颜色相同或则不相同，对于\(i+1\)的post的颜色的选择情况有不同的影响，用1个int sameColor 记录当前post和前1个post颜色相同的情况的数量，int diffColor记录当前post和前1个post颜色不同的情况的数量，初始化是对于 i = 0，i = 1的2个post，sameColor = k, diffColor = k\*\(k-1\)。则 对于 \(i + 1\)th post，其newSameColor = diffColor , newDiffColor = \(sameColor + diffColor\) \* \(k-1\)， 依次推算到 n 个post的情况。

### Code

```java
public class Solution {
    public int numWays(int n, int k) {
        if(n == 0 || k == 0) return 0;
        if(n == 1) return k;
        
        int same = k;
        int diff = k * (k-1);
        for(int i = 2; i < n; i ++) {
            int temp = diff;
            diff = (same + diff) * (k-1);
            same = temp; // 如果 i-1 和 i-2已经是相同颜色，那么i不可能和i-1 再是同一颜色
        }
        return same + diff;
    }
}
```



