# 378. Kth Smallest Element in a Sorted Matrix

> Given anxnmatrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.
>
> Note that it is the kth smallest element in the sorted order, not the kth distinct element.
>
> **Example:**
>
> ```
> matrix = [
>    [ 1,  5,  9],
>    [10, 11, 13],
>    [12, 13, 15]
> ],
> k = 8,
>
> return 13.
> ```
>
> **Note:**
>
> You may assume k is always valid, 1 ≤ k ≤ n^2

### 思路

1. 一开始看题目，因为matrix里面每一个row里面是从左到右是sorted的，每一column里面从上到下是sorted的。所以越往“左上”，数字越小；越往“右下”，数字越大。最开始最容易想到的方法就是一个一个从最小的往更大的依次寻找，一直到找到第K个元素即可。
2. 显而易见，最小的元素是左上角，即元素\(0, 0\)。然后找2nd 小的元素，即从最小的元素往右边一个，或则往下边一个走，即比较\(0, 1\) 和 \(1, 0\)的元素大小，较小者就是 2nd 小的元素。比如\(0, 1\) 较小，那么作为“下一个较小元素”的候选者就是\(0, 2\)和\(1, 1\) 中的较小者和\(1, 0\) 比较，取较小值即可。依次往后推进，但是这个方法有2个小问题。
   1. 写代码感觉有点繁琐，因为每次被取出的元素，往后面推进的时候，坐标会变换，而且需要比较才知道是其右边一个元素更小，还是下面一个元素更小。
   2. 这个方面在逻辑上有bug，还不全面。比如\(0, 1\) = 5 已经被取出作为第2小的元素，然后其右边的元素比下面的元素小，所以取出9。这时候需要找比9大的下一个元素，因为9右边没有元素，则取其下面的13 来和 \(1, 0\) = 10 比较。其实是不对的，因为\(1, 1\) = 11 比 \(1, 2\) = 13 更小，应该取11 和 10 比较才对。
3. **注意这里又是找第 K 最大/最小 值问题，往往可以往heap上面想**。上面的思路没有问题，就是往右边和下面来依次找元素，但是貌似不能只从\(0, 0\) 开始找。那么可以想最小元素肯定在第一row，所以直接把第一row 放入一个 size = n 的 min heap，pop 出来最小值。然后找小一个最小值的时候，则需要将pop出来的值的“往下走”一个的元素继续放入min heap中，因为此时它也有资格“竞争” 下一个最小值了。如此一直pop出来K个值为止，最后一个值便是结果。

### Code

```java
class Solution {
    
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        Queue<int[]> minHeap = new PriorityQueue<int[]>(new Comparator<int[]>(){
            public int compare(int[] a, int[] b) {
                return matrix[a[0]][a[1]] - matrix[b[0]][b[1]];
            }
        });
        
        for(int i = 0; i < n; i ++) minHeap.add(new int[]{0, i});
        while(k -- > 1) {
            int[] cur = minHeap.poll();
            if(cur[0] == n - 1) continue;
            minHeap.add(new int[]{cur[0]+1, cur[1]});
        }
        int[] rst = minHeap.peek();
        return matrix[rst[0]][rst[1]];
    }
}
```



