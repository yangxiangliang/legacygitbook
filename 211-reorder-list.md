# 143. Reorder List

> Given a singly linked listL:L0→L1→…→Ln-1→Ln,  
> reorder it to:L0→Ln→L1→Ln-1→L2→Ln-2→…
>
> You must do this in-place without altering the nodes' values.
>
> For example,  
> Given`{1,2,3,4}`, reorder it to`{1,4,2,3}`.



此题分为3个steps：

1. find middle node ; 2. reverse second half list after middle node; 3. merge those 2 lists

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
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) return;
        ListNode middle = findMiddle(head);
        ListNode secondHead = middle.next;
        // 坑：middle.next = null, 断开两端List
        middle.next = null;
        merge(head, reverse(secondHead));
    }
    public ListNode findMiddle(ListNode head) {
        ListNode slow = head, fast = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
    public ListNode reverse(ListNode head) {
        ListNode pre = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
    public void merge(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(0);
        while (l1 != null && l2 != null) {
            head.next = l1;
            l1 = l1.next;
            head = head.next;
            head.next = l2;
            l2 = l2.next;
            head = head.next;
        }
        // 此题目中，L1长度只会大于等于L2，所以只用考虑l1 != null, 而l2 已经是null 的情况
        if (l1 != null) {
            head.next = l1;
        }
        return ;
    }
}
```



