# 699. Falling Squares

> On an infinite number line \(x-axis\), we drop given squares in the order they are given.
>
> The`i`-th square dropped \(`positions[i] = (left, side_length)`\) is a square with the left-most point being`positions[i][0]`and sidelength`positions[i][1]`.
>
> The square is dropped with the bottom edge parallel to the number line, and from a higher height than all currently landed squares. We wait for each square to stick before dropping the next.
>
> The squares are infinitely sticky on their bottom edge, and will remain fixed to any positive length surface they touch \(either the number line or another square\). Squares dropped adjacent to each other will not stick together prematurely.
>
> Return a list`ans`of heights. Each height`ans[i]`represents the current highest height of any square we have dropped, after dropping squares represented by`positions[0], positions[1], ..., positions[i]`.
>
> **Example 1:**
>
> ```
> Input: [[1, 2], [2, 3], [6, 1]]
> Output: [2, 5, 5]
> Explanation:
>
> After the first drop of positions[0] = [1, 2]:
> _aa
> _aa
> -------
> The maximum height of any square is 2.
>
>
> After the second drop of positions[1] = [2, 3]:
> __aaa
> __aaa
> __aaa
> _aa__
> _aa__
> --------------
> The maximum height of any square is 5.  
> The larger square stays on top of the smaller square despite where its center
> of gravity is, because squares are infinitely sticky on their bottom edge.
>
>
> After the third drop of positions[1] = [6, 1]:
> __aaa
> __aaa
> __aaa
> _aa**
> _aa___a
> --------------
> The maximum height of any square is still 5.
>
> Thus, we return an answer of [2, 5, 5].
> ```
>
> **Example 2:**
>
> ```
> Input: [[100, 100], [200, 100]]
> Output: [100, 100]
> Explanation: Adjacent squares don't get stuck prematurely - only their bottom edge can stick to surfaces.
> ```
>
> **Note:**
>
> * 1 &lt;= positions.length &lt;= 1000
> * 1 &lt;= positions\[i\]\[0\] &lt;= 10^8
> * 1 &lt;= positions\[i\]\[1\] &lt;= 10^6

### 思路

1. 注意细节：这里对于position\[1, 2\]，说明该square左下角的横坐标为1，长度为2，那么认为右下角的横坐标是2，而不是3，因为从左下角1到右下角2的长度刚好是2。那么这个square的形状就是横坐标从1到2，高度为2的样子。
2. 如果后面的square的横坐标是在x, y之间，那么这时候\(x, y\)之间可能已经有其他的square了，新的square必须重叠在他们上面。所以找到\(x, y\)最高的square的位置，然后叠加上新的square的高度，便是该square稳定后的高度h了。**此时还不能把h直接加入最后结果中，因为结果中要求的是加入当前所有square中的最高高度，不一定是h\(坑\)。**
3. 到这里的时候便想到需要一个结构来表示该square，里面包含square的左下角start点，右下角end点和高度。下面用Interval表示该class，因为其形状很像interval。所以每次加入新的interval后，需要scan当前所有的intervals，如果有intervals和其overlap，那么新interval 高度便是overlap中最高的高度再加上新interval自己的高度即可。

### 复杂度

Time: O\(N^2\)

**优化**: 因为肯定要把所有 intervals scan一遍，时间复杂度至少是O\(N\)。关键是怎么优化scan当前所有intervals这一步，可以想办法把其变成O\(logN\)。能想到的办法就是把已有 的intervals 按照start point排序存放，然后用binary search来找与new interval重合的interval。

### Code

```java
class Solution {
    class Interval {
        int start;
        int end;
        int height;
        public Interval(int start, int end, int height) {
            this.start = start;
            this.end = end;
            this.height = height;
        }
    }

    public List<Integer> fallingSquares(int[][] positions) {
        List<Integer> rst = new ArrayList<>();
        List<Interval> intervals = new ArrayList<>();
        // heighest interval for all existing intervals
        int heighest = 0;
        for(int[] square : positions) {
            int height = square[1]; // height for current interval
            // current interval that needs to be added
            Interval toAdd = new Interval(square[0], square[0]+square[1]-1, square[1]);
            // TODO: expensive to iterate all existing intervals, need to optimize
            // TODO: using data structure to maintain intervals sorted as start point and
            // TODO: using binary search to find overlapping intervals with current interval
            for (Interval tmp : intervals) {
                if(toAdd.start > tmp.end || toAdd.end < tmp.start) continue; // no overlap
                height = Math.max(tmp.height + square[1], height);
            }
            toAdd.height = height;
            intervals.add(toAdd);
            // 新interval的高度不一定是所有intervals中最高的，最后加入结果的是all intervals中最高的height
            heighest = Math.max(heighest, height);
            rst.add(heighest);
        }
        return rst;
    }
}
```



