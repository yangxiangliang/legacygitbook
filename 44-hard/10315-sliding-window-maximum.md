# 239. Sliding Window Maximum

> Given an array _nums_, there is a sliding window of size \_k \_which is moving from the very left of the array to the very right. You can only see the \_k \_numbers in the window. Each time the sliding window moves right by one position.
>
> For example,
>
> Given _nums_ =`[1,3,-1,-3,5,3,6,7]`, and _k _= 3.
>
> ```
> Window position                Max
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```
>
> Therefore, return the max sliding window as`[3,3,5,5,6,7]`.
>
> **Note:**
>
> You may assume \_k \_is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.
>
> **Follow up:**
>
> Could you solve it in linear time?

### 思路 1\)

普通思路，用Heap解决，但是不是linear time。

### 复杂度

时间 O\(N \* log\(K\)\)，空间 O\(K\)

### Code 1\)

```java
public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0) return new int[0];
        
        int[] rst = new int[nums.length - k + 1];
        Queue<Integer> heap = new PriorityQueue<Integer>(Collections.reverseOrder());
        for(int i = 0; i < k; i ++) heap.add(nums[i]);
        for(int i = 0; i < nums.length - k + 1; i ++) {
            rst[i] = heap.peek();
            heap.remove(nums[i]);
            if(i < nums.length - k) heap.add(nums[i+k]);
        }
        return rst;
    }
}
```

### 思路 2）双向队列

用双向队列可以在O\(N\)时间内解决这题。当我们遇到新的数时，将新的数和双向队列的末尾比较，如果末尾比新数小，则把末尾扔掉，直到该队列的末尾比新数大或者队列为空的时候才住手。这样，我们可以保证队列里的元素是从头到尾降序的，由于队列里只有窗口内的数，所以他们其实就是窗口内第一大，第二大，第三大...的数。保持队列里只有窗口内数的方法和上个解法一样，也是每来一个新的把窗口最左边的扔掉，然后把新的加进去。然而由于我们在加新数的时候，已经把很多没用的数给扔了，这样队列头部的数并不一定是窗口最左边的数。这里的技巧是，我们队列中存的是那个数在原数组中的下标，这样我们既可以直到这个数的值，也可以知道该数是不是窗口最左边的数。这里为什么时间复杂度是O\(N\)呢？因为每个数只可能被操作最多两次，一次是加入队列的时候，一次是因为有别的更大数在后面，所以被扔掉，或者因为出了窗口而被扔掉。

### 复杂度

时间\(N\)，空间O\(K\)

### Code 2\)

```java
public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0) return new int[0];
        int[] rst = new int[nums.length - k + 1];
        Deque<Integer> queue = new ArrayDeque();
        
        for(int i = 0; i < nums.length; i ++) {
            // 每当新数进来时，如果发现队列头部的数的下标，是窗口最左边数的下标，则扔掉
            if(!queue.isEmpty() && queue.peek() == i - k) queue.pollFirst();
            // 把队列尾部所有比新数小的都扔掉，保证队列是降序的
            while(!queue.isEmpty() && nums[queue.peekLast()] < nums[i]) queue.pollLast();
            // 加入新数
            queue.addLast(i);
            // 队列头部就是该窗口内第一大的
            if(i >= k - 1) rst[i - k + 1] = nums[queue.peek()];
        }
        return rst;
    }
}
```



