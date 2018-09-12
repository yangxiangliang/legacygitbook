# 373. Find K Pairs with Smallest Sums

> You are given two integer arrays **nums1 **and **nums2 **sorted in ascending order and an integer **k**.
>
> Define a pair**\(u,v\) **which consists of one element from the first array and one element from the second array.
>
> Find the k pairs **\(u1,v1\), \(u2,v2\) ...\(uk,vk\) **with the smallest sums.
>
> **Example 1:**
>
> ```
> Given nums1 = [1,7,11], nums2 = [2,4,6],  k = 3
>
> Return: [1,2],[1,4],[1,6]
>
> The first 3 pairs are returned from the sequence:
> [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
> ```
>
> **Example 2:**
>
> ```
> Given nums1 = [1,1,2], nums2 = [1,2,3],  k = 2
>
> Return: [1,1],[1,1]
>
> The first 2 pairs are returned from the sequence:
> [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
> ```
>
> **Example 3:**
>
> ```
> Given nums1 = [1,2], nums2 = [3],  k = 3 
>
> Return: [1,3],[2,3]
>
> All possible pairs are returned from the sequence:
> [1,3],[2,3]
> ```

### 思路

* 此题找和最小的K种组合，此种"最小的K个值"之类的题目容易想到使用heap数据结构。题目关键是看怎么样找出这K种组合，不用遍历2个array中所有元素的组合，从而来提高efficient。
* 仔细想，因为nums1, nums2都是sorted的，那么和最小的K个pair只可能在nums1\[0, 1, 2, ...., min\(k, nums1.length\) - 1\]和nums2\[0, 1, 2, ....., min\(k, nums2.length\) - 1\] 中组合产生。一开始将nums1\[0, 1, 2, ..., min\(k, nums1.length\) -1 \]和 nums2\[0\] 放入heap中，注意放入heap中的是元素在array中的index，然后poll出了pair后，将\(pair\[0\], pair\[1\]+1\) 放入heap即可。因为\(pair\[0\], pair\[1\]+1\)可能会比\(pair\[0\]+1, pair\[1\]\)更小。

### 复杂度

O\(K \* log\(K\)\)，因为heap的size最多为K，而且最多有K个loop，所以复杂度和nums1, nums2的长度无关。

### Code

```java
public class Solution {
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<int[]> rst = new ArrayList();
        if(nums1.length == 0 || nums2.length == 0) return rst;
        
        Queue<int[]> heap = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {return nums1[a[0]] + nums2[a[1]] - nums1[b[0]] - nums2[b[1]];}
        });
        for(int i = 0; i < Math.min(nums1.length, k); i ++) heap.add(new int[]{i, 0});
        
        while(k -- > 0 && !heap.isEmpty()) {
            int[] cur = heap.poll();
            rst.add(new int[]{nums1[cur[0]], nums2[cur[1]]});
            if(cur[1] == nums2.length - 1) continue;
            heap.add(new int[]{cur[0], cur[1] + 1});
        }
        return rst;
    }
}
```



