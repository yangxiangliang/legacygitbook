# 265. Paint House II

> There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.
>
> The cost of painting each house with a certain color is represented by a`nxk`cost matrix. For example,`costs[0][0]`is the cost of painting house 0 with color 0;`costs[1][2]`is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.
>
> **Note:**
>
> All costs are positive integers.
>
> **Example:**
>
> ```
> Input: [[1,5,3],[2,9,4]]
> Output: 5
> Explanation: Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
>              Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5.
> ```
>
> **Follow up:**
>
> Could you solve it in O\(nk\) runtime?

### 思路 1）

1. 这里是一个 n x k 的matrix，对于第i row，第 j column paint的min cost，和第 \(i+1\) row每个paint cost是相关的，如果对于row i来说，要paint column j，那么对于 \(i+1\) row，就不能paint column j那种颜色。容易想到DP的方法，先从costs的最后row开始，往上面的row遍历计算，每一个row中paint每一种颜色的min cost。因为costs中每一个row长度为k，所以用一个长度为k的数组记录当前row paint 每一个颜色的可能的min cost 即可。

### Code 1）

```java
class Solution {
    public int minCostII(int[][] costs) {
        int len = costs.length;
        if(len == 0) return 0;
        int k = costs[0].length;
        int[] rst = new int[k]; // 记录paint 每一种颜色的min cost

        for(int i = len - 1; i >= 0; --i) {
            if(i == len - 1) {
                rst = costs[i].clone();
            } else {
                // copy 当前结果rst到 pre array, 这样rst中改变的时候，不会影响pre array改变
                int[] pre = rst.clone();

                // 记录前面row，即pre array 的最小元素和最小元素的个数
                // 因为 rst[j] = costs[i][j] + minOf(pre array except pre[j])
                // 只要pre[j] 不是 pre中的“唯一的最小元素”，那么rst[j] = costs[i][j] + minOf(pre array) 即可。
                int min = Integer.MAX_VALUE;
                int count = 0; // count of min

                for(int j = 0; j < k; j ++) {
                    if(pre[j] < min) {
                        min = pre[j];
                        count = 1;
                    } else if (pre[j] == min) {
                        count ++;
                    }
                }

                for(int j = 0; j < k; j ++) {
                    if(pre[j] == min && count == 1) {

                        int tmpMin =Integer.MAX_VALUE;
                        for(int a = 0; a < k; a ++) {
                            if(a != j) tmpMin = Math.min(tmpMin, pre[a]);
                        }
                        rst[j] = costs[i][j] + tmpMin;
                    }
                    else rst[j] = costs[i][j] + min;
                }
            }
        }

        int result = Integer.MAX_VALUE;
        for(int i = 0; i < k; i ++) result = Math.min(result, rst[i]);
        return result;
    }
}
```

### 思路 2）

1. 根据上面的分析，可以看出其实不用一个长度为k的array来记录每一个row 中paint每一个颜色的min cost。只用记录paint到上一个row中min cost的最小值和其对应的index i。paint下一个row中的时候，只要不是paint到column index i的时候，则当前cost就等于costs\[i\]\[j\] + min，如果paint到column index i的时候，因为相邻的不能paint同一种颜色，所以需要找其他剩下当中 最小值，即记录一个倒数第二小的值即可。

### Code 2\)

```java
class Solution {
    public int minCostII(int[][] costs) {
        int len = costs.length;
        if(len == 0) return 0;
        int k = costs[0].length;
        // first 是最小值, second 是倒数第二小值, minIndex表示最小值对应的column index
        int first = 0, second = 0, minIndex = -1;

        for(int i = len - 1; i >= 0; --i) {
            int f = Integer.MAX_VALUE, s = Integer.MAX_VALUE, index = 0;

            for(int j = 0; j < k; ++j) {
                int cost = costs[i][j] + (j == minIndex ? second : first);
                if(cost < f) {
                    s = f;
                    f = cost;
                    index = j;
                } else if (cost < s) {
                    // 只要cost < s，便 update tmp second min的值，即 second min 可能和 min相同
                    // 说明有好几个相同的 min 值
                    s = cost;
                }
            }

            first = f;
            second = s;
            minIndex = index;
        }
        return first;
    }
}
```



