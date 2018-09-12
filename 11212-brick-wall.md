# 554. Brick Wall

> There is a brick wall in front of you. The wall is rectangular and has several rows of bricks. The bricks have the same height but different width. You want to draw a vertical line from the **top **to the **bottom **and cross the **least **bricks.
>
> The brick wall is represented by a list of rows. Each row is a list of integers representing the width of each brick in this row from left to right.
>
> If your line go through the edge of a brick, then the brick is not considered as crossed. You need to find out how to draw the line to cross the least bricks and return the number of crossed bricks.
>
> **You cannot draw a line just along one of the two vertical edges of the wall, in which case the line will obviously cross no bricks.**
>
> Example:
>
> ```
> Input: 
> [[1,2,2,1],
>  [3,1,2],
>  [1,3,2],
>  [2,4],
>  [3,1,2],
>  [1,3,1,1]]
> Output: 2
> Explanation：
> ```

![](/assets/brick_wall.png)

> **Note:**
>
> 1. The width sum of bricks in different rows are the same and won't exceed INT\_MAX.
> 2. The number of bricks in each row is in range \[1,10,000\]. The height of wall is in range \[1,10,000\]. Total number of bricks of the wall won't exceed 20,000.

### 思路

1. 想穿过越少的 bricks，那么也就是想尽量多的穿过bricks之间的间隙。那么怎么知道bricks的间隙处在哪里。
2. 因为知道每个brick的宽度，那么容易算出每个brick的右边endpoint的位置，如果假设每个row起始点都是0，那么每一层endpoint相同的brick，就说明其间隙在同一条竖直线上。那么只用找到endpoint重复最多的个数是多少，用总的row的数量减去 该数 即可。

### Code

```java
class Solution {
    public int leastBricks(List<List<Integer>> wall) {
        // record endpoint of each brick
        HashMap<Integer, Integer> map = new HashMap();
        int max = 0;
        for(List<Integer> row : wall) {
            int end = 0;
            for(int i = 0; i < row.size() - 1; i ++) {
                end += row.get(i);
                map.put(end, map.getOrDefault(end, 0) + 1);
                max = Math.max(max, map.get(end));
            }
        }
        return wall.size() - max;
    }
}
```



