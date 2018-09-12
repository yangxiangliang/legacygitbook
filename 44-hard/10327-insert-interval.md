# 57. Insert Interval

> Given a set of _non-overlapping_ intervals, insert a new interval into the intervals \(merge if necessary\).
>
> You may assume that the intervals were initially sorted according to their start times.
>
> **Example 1:**  
> Given intervals`[1,3],[6,9]`, insert and merge`[2,5]`in as`[1,5],[6,9]`.
>
> **Example 2:**  
> Given`[1,2],[3,5],[6,7],[8,10],[12,16]`, insert and merge`[4,9]`in as`[1,2],[3,10],[12,16]`.
>
> This is because the new interval`[4,9]`overlaps with`[3,5],[6,7],[8,10]`.

### 思路

思路就按照顺序来的，因为intervals都是按照start times 排序好的。

* 先add在newInterval 左边不和其overlap的interval
* 再merge与newInterval overlap的intervals，把merge后的整个new interval加入结果中\(**注意2个interval overlap的条件**\)
* 最后add new interval右边不和其overlap的interval

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
    public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
        List<Interval> rst = new ArrayList();
        int i = 0;
        
        // add intervals at left of new interval 
        while(i < intervals.size() && intervals.get(i).end < newInterval.start) 
            rst.add(intervals.get(i++));

        // merge new interval with its overlapped intervals
        // 注意: overlap的条件必须是 current_interval.start <= newInterval.end
        // 如果 newInterval.end < current_interval.start，那么它们没有overlap
        while(i < intervals.size() && intervals.get(i).start <= newInterval.end) {
            // 注意 merge的时候，end应该取2者的较大值
            newInterval.end = Math.max(newInterval.end, intervals.get(i).end);
            // 注意merge的时候，start应该取2者的较小值
            newInterval.start = Math.min(newInterval.start, intervals.get(i).start);
            i ++;
        }
        rst.add(newInterval);
        
        // add intervals at right of new interval
        while(i < intervals.size()) rst.add(intervals.get(i++));
        return rst;
    }
}
```



