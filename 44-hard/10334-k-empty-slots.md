# 683. K Empty Slots

> There is a garden with`N`slots. In each slot, there is a flower. The`N`flowers will bloom one by one in`N`days. In each day, there will be`exactly`one flower blooming and it will be in the status of blooming since then.
>
> Given an array`flowers`consists of number from`1`to`N`. Each number in the array represents the place where the flower will open in that day.
>
> For example,`flowers[i] = x`means that the unique flower that blooms at day`i`will be at position`x`, where`i`and`x`will be in the range from`1`to`N`.
>
> Also given an integer`k`, you need to output in which day there exists two flowers in the status of blooming, and also the number of flowers between them is`k`and these flowers are not blooming.
>
> If there isn't such day, output -1.
>
> **Example 1:**
>
> ```
> Input: 
> flowers: [1,3,2]
> k: 1
> Output: 2
> Explanation: In the second day, the first and the third flower have become blooming.
> ```
>
> **Example 2:**
>
> ```
> Input: 
> flowers: [1,2,3]
> k: 1
> Output: -1
> ```

### 思路 1）

1. 题目意思就是每一天 i 都可能有位置在k的花blooming，下一天blooming 花的位置在 n。其实将blooming的花看作一个从 1到N的排序的数列，每天把当天blooming的花插入序列中，因为是排序数列，所以用binary search来寻找insert point即可。注意每次插入的时候，比如插入的position是 m, 那么只用找 position m 和其之前，之后的花的间隔是否为 k 即可。因为 m 位置插入的花，并不改变其他位置的花之间的间隔。
2. **复杂度**：Time: O\(N \* log\(N\)\)

### Code 1\)

```java
class Solution {
    public int kEmptySlots(int[] flowers, int k) {
        List<Integer> list = new ArrayList(); // list 记录当前已经bloom的flower，是sorted的
        for(int i = 0; i < flowers.length; i ++) {
            // insert 是当前flower的insertion position
            int insert = binarySearch(list, flowers[i]);
            int pre = insert - 1;
            int next = insert;
            if(pre >= 0 && pre < list.size() && flowers[i] - list.get(pre) - 1 == k) return i + 1;
            if(next < list.size() && list.get(next) - flowers[i] - 1 == k) return i + 1;
            list.add(insert, flowers[i]);
        }
        return -1;
    }
    
    private int binarySearch(List<Integer> list, int target) {
        int start = 0, end = list.size() - 1;
        
        while(start < end) {
            int mid = (start + end) / 2;
            if(list.get(mid) < target) start = mid + 1;
            else end = mid;
        }
        
        // handle corner case
        if(start < list.size() && target > list.get(start)) return start + 1;
        return start;
    }
}
```

### 思路 2）

思路和复杂度都和上面相同，只是不用自己maintain sorted list 来记录当前已经bloom的flower。因为每次插入flower 时候，需要找到比其小的最大flower，和比其大的最小flower，看他们之间的2个间隔是否是k。可以换作 **TreeSet** 来解决，因为TreeSet 内部就已经是排序的，而且容易找到距离某个值“左右”closest 的值。

### Code 2\)

```java
class Solution {
    public int kEmptySlots(int[] flowers, int k) {
        TreeSet<Integer> set = new TreeSet();
        for(int i = 0; i < flowers.length; i ++) {
            set.add(flowers[i]);
            Integer left = set.lower(flowers[i]);
            Integer right = set.higher(flowers[i]);
            if(left != null && flowers[i] - left - 1 == k || right != null && right - flowers[i] - 1 == k) return i + 1;
        }
        return -1;
    }
}
```

### 思路 3）

1. 该思路比较奇特，参考 [LeetCode 讨论](https://leetcode.com/problems/k-empty-slots/discuss/107931/JavaC++-Simple-O%28n%29-solution)
2. 首先把输入转化一下变成 days\[\], days\[i\] 表示 position \(i+1\)的花bloom的时间。那么问题就变成了找一个长度为 k 的window，该window 左边的元素days\[left\] bloom 的时间，和 其右边 的元素days\[right\] bloom 的时间，都必须小于window中任何花bloom的时间，这样就保证在max\(days\[left\], days\[right\]\) 天时，left, right 花之间有K个没有bloom 的花。然后取所有这样 window的max\(days\[left\], days\[right\]\) 的值中的最小值即可。

### 复杂度

Time： O\(N\)

### Code 3\)

```java
class Solution {
    public int kEmptySlots(int[] flowers, int k) {
        int[] days = new int[flowers.length];
        for(int i = 0; i < flowers.length; i ++) days[flowers[i] - 1] = i + 1;
        int left = 0, right = k + 1;
        
        int rst = Integer.MAX_VALUE;
        for(int i = 1; right < flowers.length; i ++) {
            if(days[i] < days[left] || days[i] < days[right] || i == right) {
                if(i == right) rst = Math.min(rst, Math.max(days[left], days[right]));
                left = i;
                right = left + k + 1;
            }
        }
        return rst == Integer.MAX_VALUE ? -1 : rst;
    }
}
```



