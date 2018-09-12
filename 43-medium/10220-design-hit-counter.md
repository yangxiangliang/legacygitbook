# 362. Design Hit Counter

> Design a hit counter which counts the number of hits received in the past 5 minutes.
>
> Each function accepts a timestamp parameter \(in seconds granularity\) and you may assume that calls are being made to the system in chronological order \(ie, the timestamp is monotonically increasing\). You may assume that the earliest timestamp starts at 1.
>
> It is possible that several hits arrive roughly at the same time.
>
> **Example: **
>
> ```
> HitCounter counter = new HitCounter();
>
> // hit at timestamp 1.
> counter.hit(1);
>
> // hit at timestamp 2.
> counter.hit(2);
>
> // hit at timestamp 3.
> counter.hit(3);
>
> // get hits at timestamp 4, should return 3.
> counter.getHits(4);
>
> // hit at timestamp 300.
> counter.hit(300);
>
> // get hits at timestamp 300, should return 4.
> counter.getHits(300);
>
> // get hits at timestamp 301, should return 3.
> counter.getHits(301);
> ```
>
> **Follow up: **
>
> What if the number of hits per second could be very large? Does your design scale?

### 思路  1）

用一个queue，里面值就是每个hit的timestamp，每次hit，就把其timestamp加入queue即可。然后getHits的时候，看当前timestamp和queue.peek\(\) 是否 &gt;= 300， 如果是，则需要queue.poll\(\)，然后return queue.size\(\) 便是结果。

### 注意

这种方法因为每次都把hit放入queue，如果同时有很多hits的时候，则queue的size会很大，然后一个一个poll出来，很不efficient。

### Code 1\)

```java
public class HitCounter {
    Queue<Integer> queue;

    /** Initialize your data structure here. */
    public HitCounter() {
        queue = new LinkedList();
    }

    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        queue.offer(timestamp);
    }

    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        while(!queue.isEmpty() && timestamp - queue.peek() >= 300) queue.poll();
        return queue.size();
    }
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter obj = new HitCounter();
 * obj.hit(timestamp);
 * int param_2 = obj.getHits(timestamp);
 */
```

### 思路  2\)

使用buckets，因为我们只用关系300 seconds之内的hits，其他的不用管了。则可以用int\[\] hits = new ints\[300\]，来记录每个timestamp的hits的number。超过300秒的timestamp，则用mod   timestamp % 300 = index of hits array。比如timestamp等于305的hits number是3，因为此时timestamp == 5的hits number已经不重要了，所以可以直接hits\[305 % 300\] = 3。但是这样的话，在getHits的时候，我们不知道hits\[5\]对应的timestamp是5, 还是305，还是605。。。 所以我们还需要另外一个int\[\] times记录每个index对应的真实的timestamp。

### Code 2\)

```java
public class HitCounter {
    int[] hits;
    int[] times;

    /** Initialize your data structure here. */
    public HitCounter() {
        hits = new int[300];
        times = new int[300];
    }

    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        int index = timestamp % 300;
        if(times[index] != timestamp) {
            times[index] = timestamp;
            hits[index] = 1;
        } else hits[index] ++;
    }

    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        int total = 0;
        for(int i = 0; i < 300; i ++) {
            if(timestamp - times[i] < 300) total += hits[i];
        }
        return total;
    }
}
```



