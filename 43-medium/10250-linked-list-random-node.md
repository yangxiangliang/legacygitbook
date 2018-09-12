# 382. Linked List Random Node

> Given a singly linked list, return a random node's value from the linked list. Each node must have the **same probability **of being chosen.
>
> **Follow up:**
>
> What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?
>
> **Example:**
>
> ```
> // Init a singly linked list [1,2,3].
> ListNode head = new ListNode(1);
> head.next = new ListNode(2);
> head.next.next = new ListNode(3);
> Solution solution = new Solution(head);
>
> // getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
> solution.getRandom();
> ```

### 思路 1\)

一开始就是最简单的想法，就是用1个HashMap记录每个node的index和其对应的value。head的index为0，然后其他node的index依次 +1。这样hashMap的size也就是所有node的个数，用个random函数就可以随机找出一个index。但是这个方法显然不符合 **Follow up  **的要求，要到达 **Follow up **的要求，需要用到 **Reservoir Sampling，**见下面思路 2\)。

### Code 1\)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    HashMap<Integer, Integer> map;
    java.util.Random rand;

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    public Solution(ListNode head) {
        rand = new java.util.Random();
        map = new HashMap();
        int index = 0;
        while(head != null) {
            map.put(index++, head.val);
            head = head.next;
        }
    }

    /** Returns a random node's value. */
    public int getRandom() {
        return map.get(rand.nextInt(map.size()));
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```

### 思路 2）Reservoir Sampling

[蓄水池抽样方法参考](http://blog.jobbole.com/42550/)

大体意思就是假设已经有\(n-1\)的元素，抽到每个元素的概率是平均的，即 1/\(n-1\)。当加入第n个元素的时候，只用保证抽到第n个元素的概率为 1/n，那么便可以保证抽到这n个元素中任何一个的概率都是 1/n。

证明：因为已经保证抽到第n个元素的概率为1/n了，那么研究抽到前\(n-1\)个元素任何一个的概率，因为前\(n-1\)被抽到的概率也是平均的，这里抽到前\(n-1\)的概率为\(n-1\)/n，那么抽到其中任何一个概率为 \(n-1\)/n  \* 1/\(n-1\) = 1/n，符合要求。

### 复杂度

时间O\(N\), 空间 O\(1\)

### Code 2\)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    ListNode head;
    java.util.Random rand;

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    public Solution(ListNode head) {
        rand = new java.util.Random();
        this.head = head;
    }

    /** Returns a random node's value. */
    public int getRandom() {
        int rst = 0;
        // 每次 getRandom()的时候，都要移动 head从头到尾，所以用node来做移动，head一直放在list的头部位置
        ListNode node = head;
        for(int i = 0; node != null; i ++) {
            if(rand.nextInt(i+1) == i) rst = node.val;
            node = node.next;
        }
        return rst;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```



