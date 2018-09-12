# 774. Minimize Max Distance to Gas Station

> On a horizontal number line, we have gas stations at positions `stations[0], stations[1], ..., stations[N-1]`, where`N = stations.length`.
>
> Now, we add`K`more gas stations so that **D**, the maximum distance between adjacent gas stations, is minimized.
>
> Return the smallest possible value of **D**.
>
> **Example:**
>
> ```
> Input: stations = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], K = 9
> Output: 0.500000
> ```
>
> **Note:**
>
> 1. stations\[i\] will be an integer in range\[0, 10^8\].
> 2. Answers within 10^-6 of the true value will be accepted as correct.

### 思路 1）

1. 因为需要把所有间隔的最大间隔尽可能小。那么在插入新的gas station的时候，应该往当前平均间隔最大的那一段区域插入station。那么每次需要得到最大间隔的区域段，可以想到用maxHeap。
2. 这里需要注意插入heap的元素应该是 int\[2\] node, node\[0\] 表示original该段间隔的长度，node\[1\] 表示该段间隔数目，那么该区域插入stations后每个间隔的长度就是 \(double\)node\[0\] / node\[1\] .
3. 每次往间隔长度最大的区域中加入新的gas station 直到加入了K个gas station 即可。

### 复杂度

Time: O\(\(K + N\)\*log\(N\)\)

### Code 1\)

```java
class Solution {
    public double minmaxGasDist(int[] stations, int K) {
        PriorityQueue<int[]> maxHeap = new PriorityQueue<int[]>(new Comparator<int[]>(){
            public int compare(int[] a, int [] b) {
                return (double)a[0] / a[1] < (double)b[0] / b[1] ? 1 : -1;
            }
        });

        for(int i = 1; i < stations.length; i ++) {
            maxHeap.offer(new int[]{stations[i] - stations[i-1], 1});
        }

        for(int i = 0; i < K; i ++) {
            int[] cur = maxHeap.poll();
            cur[1] ++;
            maxHeap.offer(cur);
        }

        int[] rst = maxHeap.poll();
        return (double)rst[0] / rst[1];
    }
}
```

### 思路 2）

1. 因为这里知道stations\[i\] 位置的最大值为 10^8。那么可不可以直接找可能的最大间隔，而且需要这个间隔尽可能小。可以用binary search来找。
2. 那么需要一个函数来判断当前的间隔值是否可能通过增加K个gas station 来得到。

### Code 2\)

```java
class Solution {
    public double minmaxGasDist(int[] stations, int K) {
        double low = 0.0, high = 1e8;
        while(high - low > 1e-6) {
            double mid = (low + high) / 2;
            // 如果mid 可能，那么看能不能找到更小的间隔值；反之需要在更大的范围里面找间隔值
            if(possible(mid, stations, K)) {
                high = mid;
            } else low = mid;
        }
        return low;
    }

    private boolean possible(double d, int[] stations, int k) {
        int num = 0; // 表示至少需要 num 个gas station 加入其中才能让所有的gas station 价格为 d
        for(int i = 1; i < stations.length; ++i) {
            num += (int) (stations[i] - stations[i-1]) / d;
        }
        return num <= k;
    }
}
```



