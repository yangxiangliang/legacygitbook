# 759. Employee Free Time

> We are given a list`schedule`of employees, which represents the working time for each employee.
>
> Each employee has a list of non-overlapping`Intervals`, and these intervals are in sorted order.
>
> Return the list of finite intervals representing **common, positive-length free time **for all employees, also in sorted order.
>
> **Example 1:**
>
> ```
> Input: schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]
> Output: [[3,4]]
> Explanation:
> There are a total of three employees, and all common
> free time intervals would be [-inf, 1], [3, 4], [10, inf].
> We discard any intervals that contain inf as they aren't finite.
> ```
>
> **Example 2:**
>
> ```
> Input: schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]
> Output: [[5,6],[7,9]]
> ```
>
> \(Even though we are representing`Intervals`in the form`[x, y]`, the objects inside are`Intervals`, not lists or arrays. For example,`schedule[0][0].start = 1, schedule[0][0].end = 2`, and`schedule[0][0][0]`is not defined.\)
>
> Also, we wouldn't include intervals like \[5, 5\] in our answer, as they have zero length.
>
> **Note:**
>
> 1. schedule and schedule\[i\] are lists with lengths in range \[1, 50\].
> 2. 0 &lt;= schedule\[i\].start &lt; schedule\[i\].end &lt;= 10^8

### 思路

1. 因为这里需要求所有employee的free time。即结果中free time的interval不能和schedule中的任何一个interval相互交叉。
2. 那么用intervals题目经典做法：把schedule中所有的interval按照其start point来sort，然后寻找所有intervals之间的空闲的gap即可。
3. **注意：**在把所有schedule sort好之后，在找free的gap时候，需要记录当前最大的end值，应该把该end值和下一个sorted的interval的start值相比较即可。如果end小于下一个interval的start值，则找到一个free的gap。注意当前最大的end值不是前一个interval的end值，因为所有intervals只是按照start point来sort的。即intervals\[i\]的end值和intervals\[i+1\]的end值的大小，无法判断，只能用一个值来时刻记录当前遍历过的intervals中的最大的end值。

### 复杂度

时间: O\(n \* log\(n\)\)，n 是所有schedule的个数，主要 complexity就是sort all schedules的时间

空间：O\(n\)，n 是所有schedule的个数

### Code

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> allIntervals = new ArrayList<>();
        for(List<Interval> list : schedule) {
            for(Interval interval : list) {
                allIntervals.add(interval);
            }
        }

        Collections.sort(allIntervals, new Comparator<Interval>() {
            public int compare(Interval a, Interval b) {return a.start - b.start;}
        });

        // 记录遍历到当前interval的最大end值
        int end = allIntervals.get(0).end;
        List<Interval> rst = new ArrayList<>();
        for(int i = 1; i < allIntervals.size(); i ++) {
            Interval cur = allIntervals.get(i);
            if(end < cur.start) {
                rst.add(new Interval(end, cur.start));
                end = cur.end;
            } else {
                end = Math.max(end, cur.end);
            }
        }
        return rst;
    }
}
```



