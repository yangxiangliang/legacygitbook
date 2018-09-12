# 56. Merge Intervals

> Given a collection of intervals, merge all overlapping intervals.
>
> **For example, **
>
> Given `[1,3],[2,6],[8,10],[15,18]`,
>
> return `[1,6],[8,10],[15,18]`.

### 思路

首先根据interval的start point从小到大sort 所有的interval。然后用int start，end 记录需要加入result list的interval的2个端点。按顺序遍历sorted的intervals，如果\(i+1\).start &lt;= i.end，则这2个interval需要merge成1个，即end = Math.max\(i.end, \(i+1\).end\)；反之则将 new Interval\(start, end\) 加入result中，然后Set  start, end 值为\(i+1\) 个 Interval的start和end值，直到遍历结束。

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
public class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        Collections.sort(intervals, new Comparator<Interval>() {
            public int compare(Interval i1, Interval i2) {return i1.start - i2.start;}
        });

        List<Interval> rst = new ArrayList();
        if(intervals == null || intervals.size() == 0) return rst;

        // 初始start, end为first interval的值
        int start = intervals.get(0).start;
        int end = intervals.get(0).end;
        for(int i = 1; i < intervals.size(); i ++) {
            if(intervals.get(i).start <= end) end = Math.max(end, intervals.get(i).end); // 这里取previous end和当前interval的end的最大值
            else {
                rst.add(new Interval(start, end));
                start = intervals.get(i).start;
                end = intervals.get(i).end;
            }
        }
        // add 最后1个interval
        rst.add(new Interval(start, end));
        return rst;
    }
}
```



