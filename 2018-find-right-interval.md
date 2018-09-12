# 436. Find Right Interval

> Given a set of intervals, for each of the interval i, check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, which can be called that j is on the "right" of i.
>
> For any interval i, you need to store the minimum interval j's index, which means that the interval j has the minimum start point to build the "right" relationship for interval i. If the interval j doesn't exist, store -1 for the interval i. Finally, you need output the stored value of each interval as an array.
>
> **Note:**
>
> 1. You may assume the interval's end point is always bigger than its start point.
> 2. You may assume none of these intervals have the same start point.
>
> **Example 1:**
>
> ```
> Input: [ [1,2] ]
>
> Output: [-1]
>
> Explanation: There is only one interval in the collection, so it outputs -1.
> ```
>
> **Example 2:**
>
> ```
> Input: [ [3,4], [2,3], [1,2] ]
>
> Output: [-1, 0, 1]
>
> Explanation: There is no satisfied "right" interval for [3,4].
> For [2,3], the interval [3,4] has minimum-"right" start point;
> For [1,2], the interval [2,3] has minimum-"right" start point.
> ```
>
> **Example 3:**
>
> ```
> Input: [ [1,4], [2,3], [3,4] ]
>
> Output: [-1, 2, -1]
>
> Explanation: There is no satisfied "right" interval for [1,4] and [3,4].
> For [2,3], the interval [3,4] has minimum-"right" start point.
> ```

### 思路

该题目就是找start点在 interval i 的 end 点右边或则相等的最近的那个interval的index值。**该种问题，“找大于或等于某个点的最小值”， “找小于或等于某个点的最大值”。可以想到Java里面的TreeMap来解决。用里面的ceilingEntry\(\), floorEntry\(\) methods**

### 复杂度

Time: O\(N\*log\(N\)\)，因为TreeMap每次操作时log\(N\)

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
    public int[] findRightInterval(Interval[] intervals) {
        int[] rst = new int[intervals.length];
        TreeMap<Integer, Integer> map = new TreeMap();
        for(int i = 0; i < intervals.length; i ++) map.put(intervals[i].start, i);
        for(int i = 0; i < intervals.length; i ++) {
            Map.Entry<Integer, Integer> right = map.ceilingEntry(intervals[i].end);
            if(right != null) rst[i] = right.getValue();
            else rst[i] = -1;
        }
        return rst;
    }
}
```

### 思路 2）

1. 想这题目有没有可能有O\(N\)的解法。因为要找某个interval end点的右边最近的start点的interval，显然将所有intervals排序后会比较方便查找。但是如果想用排序又在O\(N\)解决，那么想到buckets sort会不会用于该题中。
2. 用buckets sort，那么需要知道buckets总长度，这里便是找出所有intervals的start, end点中的最大和最小值，buckets的长度就是\(max - min + 1\)。因为没有buckets的start点是重复的，那么buckets\[i\]的值可以赋值为start点为\(i+min\)的interval的index值，如果没有该start值的interval，可以暂用-1表示。
3. 然后，就是想怎么才能用buckets\[i\]表示“坐标点”为i\(这里把min看作坐标点0，max看作坐标点的尾端\)处右边或等于i的最小start值的interval index。那么可以从buckets右边往左边scan，如果buckets\[i\]有值，那么不变，该interval index便是所找的结果。如果buckets\[i\] = -1, 说明没有start point落在该坐标点上，可以赋值为bucket\[i\] = bucket\[i+1\]，这样来传递其右边最小start值的interval index。

### 复杂度

Time: O\(N\)

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
    public int[] findRightInterval(Interval[] intervals) {
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for(Interval i : intervals) {
            min = Math.min(min, i.start);
            max = Math.max(max, i.end);
        }
        int[] buckets = new int[max - min + 1];
        Arrays.fill(buckets, -1); // 填上-1，区分哪些bucket会被赋值，哪些没有被赋值
        for(int i = 0; i < intervals.length; i ++) {
            buckets[intervals[i].start - min] = i;
        }
        // 传递buckets[i]值，buckets[i]的值表示"坐标"为i的点其右边或等于i的最小start点的interval的index值
        for(int i = buckets.length - 2; i >= 0; i--) {
            if(buckets[i] == -1) buckets[i] = buckets[i+1];
        }
        int[] rst = new int[intervals.length];
        for(int i = 0; i < intervals.length; i ++) rst[i] = buckets[intervals[i].end - min];
        return rst;
    }
}
```



