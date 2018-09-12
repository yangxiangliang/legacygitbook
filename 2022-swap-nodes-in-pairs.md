# 24. Swap Nodes in Pairs

> Given a linked list, swap every two adjacent nodes and return its head.
>
> For example,  
> Given`1->2->3->4`, you should return the list as`2->1->4->3`.
>
> Your algorithm should use only constant space. You may **not **modify the values in the list, only nodes itself can be changed.

### 思路 1\)

Recursion 方法处理

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
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode next = head.next.next;
        ListNode newHead = head.next;
        newHead.next = head;
        head.next = swapPairs(next);
        return newHead;
    }
}
```

### Code  2\)

iterative的方法。用ListNode pre 记录上一段swap的2个nodes中靠后面的node。这样用于 “拼接” 下一段swap nodes中的第一个node。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;

        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;

        while(head != null && head.next != null) {
            ListNode next = head.next;
            head.next = head.next.next;
            next.next = head;
            pre.next = next;
            pre = head;
            head = head.next;
        }
        return dummy.next;
    }
}
```



