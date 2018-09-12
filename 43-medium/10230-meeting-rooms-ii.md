# 253. Meeting Rooms II

> Given an array of meeting time intervals consisting of start and end times`[[s1,e1],[s2,e2],...]`\(si&lt; ei\), find the minimum number of conference rooms required.
>
> **For example,**  
> Given`[[0, 30],[5, 10],[15, 20]]`,  
> return`2`.

### 思路

按照interval的start time来sort interval，然后从start time 小到大来遍历interval。需要找到不overlap的interval，因为它们可以合用1个room，或则说可以合并成1个更大的interval。为了尽量能找到可以合并的interval，需要先找end time尽量小的interval，这样才尽可能的没有overlap，所以可以用min heap来维护interval的end time，每次取出的都是end time最小的interval，看能不能和其合并为1个大的interval\(即 合用1个room\)。

### 复杂度

时间 O\(N\*log\(N\)\)

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
 // Sort array intervals by start time
public class Solution {
    public int minMeetingRooms(Interval[] intervals) {
        if (intervals == null || intervals.length == 0) {
            return 0;
        }
        Arrays.sort(intervals, new Comparator<Interval>() {
            public int compare(Interval a, Interval b) { return a.start - b.start; }
        });

        // Use a min Heap to maintain min end time interval
        PriorityQueue<Interval> rst = new PriorityQueue<Interval>(intervals.length, new Comparator<Interval>(){
            public int compare(Interval a, Interval b) { return a.end - b.end; }
        });
        rst.offer(intervals[0]);

        for (int i=1; i<intervals.length; i++) {
            Interval interval = rst.poll();
            if (intervals[i].start >= interval.end) {
                // 相当于把intervals[i]和interval合并到1个room里，因为没有overlap
                // 从这里也可以看出为什么用min heap维护end time interval，因为这样保证每次poll的interval的end time尽量小
                // 才尽可能和后面的interval公用1个room
                interval.end = intervals[i].end;
            } else {
                // 和interval有overlap，无法合并到1个room里
                rst.offer(intervals[i]);
            }
            rst.offer(interval);
        }
        return rst.size();
    }
}
```

### 思路 2）

时间复杂度和上面一样，都是 O\(N \* log\(N\)\)。但是写法更简单，不用priority queue。

把所有的start，end的时间拿出来sort，然后一个一个比较。如果当前 start &lt; end，可以理解为当前meeting 没有结束，那么需要增加一个room；反之则移动end pointer去找下一个最小的meeting end time。

### Code 2\)

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
    public int minMeetingRooms(Interval[] intervals) {
        int[] starts = new int[intervals.length]; // records for all start points
        int[] ends = new int[intervals.length]; // records for all end points
        
        for(int i = 0; i < intervals.length; i ++) {
            starts[i] = intervals[i].start;
            ends[i] = intervals[i].end;
        }
        Arrays.sort(starts);
        Arrays.sort(ends);
        
        int rooms = 0, end = 0;
        for(int i = 0; i < starts.length; i ++){
            // means current meeting does not end, we need a new room for new meeting
            if(starts[i] < ends[end]) rooms ++;
            else end ++;
        }
        return rooms;
    }
}
```



