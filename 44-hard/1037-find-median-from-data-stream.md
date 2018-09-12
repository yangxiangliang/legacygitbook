# 295. Find Median from Data Stream

> Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.
>
> **Examples:**
>
> `[2,3,4]`, the median is`3`
>
> `[2,3]`, the median is`(2 + 3) / 2 = 2.5`
>
> Design a data structure that supports the following two operations:
>
> * void addNum\(int num\) - Add a integer number from the data stream to the data structure.
> * double findMedian\(\) - Return the median of all elements so far.
>
> **For example:**
>
> ```
> addNum(1)
> addNum(2)
> findMedian() -> 1.5
> addNum(3) 
> findMedian() -> 2
> ```

### 思路

这道题用2个heap，一个max heap 记录ordered integer list的前半段，一个min heap记录后半段，可以让max heap 的 size等于min heap的size 或则比其大1。如果2个heap的size相等，那么取2个heap的peek 除以2，如果max heap的size大1，那么直接return max heap的peek值即可。

### Code 1\)

** 不简洁的写法**

```java
public class MedianFinder {
    Queue<Integer> maxHeap;
    Queue<Integer> minHeap;

    /** initialize your data structure here. */
    public MedianFinder() {
        maxHeap = new PriorityQueue<Integer>(new Comparator<Integer>() {
            public int compare(Integer a, Integer b) { return b - a; }
        });
        minHeap = new PriorityQueue<Integer>(new Comparator<Integer>() {
           public int compare(Integer a, Integer b) {return a - b; } 
        });
    }

    public void addNum(int num) {
        // let maxHeap.size() == minHeap.size() 或则 maxHeap.size() == minHeap.size() + 1
        if(maxHeap.isEmpty() && minHeap.isEmpty()) maxHeap.add(num);
        else if(!maxHeap.isEmpty()) {
            if(num <= maxHeap.peek()) maxHeap.add(num);
            else minHeap.add(num);
        }
        else if(!minHeap.isEmpty()) {
            if(num >= minHeap.peek()) minHeap.add(num);
            else maxHeap.add(num);
        }

        if(maxHeap.size() - minHeap.size() == 2) minHeap.add(maxHeap.poll());
        else if(minHeap.size() - maxHeap.size() == 2) maxHeap.add(minHeap.poll());
    }

    public double findMedian() {
        if(maxHeap.size() - minHeap.size() == 1) return (double) maxHeap.peek();
        if(minHeap.size() - maxHeap.size() == 1 ) return (double) minHeap.peek();
        return (maxHeap.peek() + minHeap.peek()) / (double) 2;
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

### Code 2\)

**简洁的写法**

因为默认让max heap的size等于min heap或则比min heap 大1，当 add number的时候，先加入max heap，然后作minHeap.add\(maxHeap.poll\(\)\)，相当于把max heap的最大数放到后半段。如果此时min heap的size大于max heap，那么再把min heap中最小的放入前半段，即 maxHeap.add\(minHeap.poll\(\)\)。

```java
public class MedianFinder {
    // 注意：Heap 以及comparator的简洁写法，可以不 initialize heap size
    Queue<Integer> minHeap = new PriorityQueue();
    Queue<Integer> maxHeap = new PriorityQueue<Integer>(Collections.reverseOrder());

    /** initialize your data structure here. */
    public MedianFinder() {}
    
    public void addNum(int num) {
        // let maxHeap.size() == minHeap.size() 或则 maxHeap.size() == minHeap.size() + 1
        maxHeap.add(num);
        minHeap.add(maxHeap.poll());
        if(maxHeap.size() < minHeap.size()) maxHeap.add(minHeap.poll());
    }
    
    public double findMedian() {
        if(maxHeap.size() > minHeap.size()) return (double) maxHeap.peek();
        return (maxHeap.peek() + minHeap.peek()) / (double) 2;
    }
}
```









