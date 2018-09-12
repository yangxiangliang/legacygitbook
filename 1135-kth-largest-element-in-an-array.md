# 215. Kth Largest Element in an Array

> Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
>
> For example,
>
> Given`[3,2,1,5,6,4]`and k = 2, return 5.
>
> **Note:**
>
> You may assume k is always valid, 1 ≤ k ≤ array's length.

### 思路

1. 求 Kth 最大值，首先就想到用Heap结果，可以maintain一个 k size的min heap即可。时间复杂度是 O\(n \* log k\)
2. 想想有没有更好的方法，求 Kth 最大值，和排序相关。排序比log更快的，容易想到buckets sort，用buckets sort 也能找到 kth最大值，时间复杂度是O\(n\)

### Code 1\) Heap

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Queue<Integer> heap = new PriorityQueue<Integer>();

        for(int num : nums) {
            heap.add(num);
            if(heap.size() > k) heap.poll();
        }
        return heap.peek();
    }
}
```

### Code 2\) Bucket Sort

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        // bucket sort, 找nums中的min, max value，来确定buckets的长度
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for(int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        int[] buckets = new int[max - min + 1];
        for(int num : nums) buckets[num - min] ++;
        int count = 0;
        for(int i = buckets.length - 1; i >= 0; i --) {
            count += buckets[i];
            if(count >= k) return i + min;
        }
        return -1;
    }
}
```

### Code 3\) Quick Select 思路

Time: Average O\(N\), Worst case O\(N^2\)

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return partition(nums, 0, nums.length - 1, k);
    }
    
    private int partition (int[] nums, int start, int end, int k) {
        int pivot = nums[start];
        swap(nums, start, end);
        
        int insert = start;
        for(int i = start; i < end; i ++) {
            if(nums[i] > pivot) swap(nums, i, insert++);
        }
        
        swap(nums, insert, end);
        if(insert + 1 == k) return pivot;
        else if(insert + 1 < k) return partition(nums, insert + 1, end, k);
        else return partition(nums, start, insert - 1, k);
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```



