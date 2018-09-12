# 61. Rotate List

> Given a list, rotate the list to the right by k places, where k is non-negative.
>
> For example:
>
> Given 1-&gt;2-&gt;3-&gt;4-&gt;5-&gt;NULL, and k = 2
>
> return 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL



这个题目tricky的地方是如果 k &gt; length\(List\), 相当于 k = k % length\(List\) 来rotate。面试的时候一开始需要和面试官确认这个点是否可以这么处理！然后利用slow, fast 指针来找到需要rotate的node。

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) return head;
        int length = getLength(head);
        k = k % length;
        // special case, if k == 0, no rotation for the list
        if (k == 0) return head;
        ListNode slow = head, fast = head;
        for (int i = 0; i < k; i++) {
            fast = fast.next;
        }
        while(fast.next != null) {
            slow = slow.next;
            fast = fast.next;
        }
        ListNode newHead = slow.next;
        slow.next = null;
        fast.next = head;
        return newHead;
    }
    
    public int getLength(ListNode head) {
        int count = 0;
        while(head != null) {
            count += 1;
            head = head.next;
        }
        return count;
    }
}
```



