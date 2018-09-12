# 19. Remove Nth Node From End of List

> Given a linked list, remove the  Nth node from the end of list and return its head.
>
> For example,
>
>    Given linked list: 1-&gt;2-&gt;3-&gt;4-&gt;5, and n = 2.
>
>    After removing the second node from the end, the linked list becomes 1-&gt;2-&gt;3-&gt;5.
>
> **Note:**
>
> Given n will always be valid.
>
> Try to do this in one pass.

1\) 比较容易想到的办法，用一个hashMap记录每个node的位置，one pass后再选择应该被remove掉的node

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        HashMap<Integer, ListNode> map = new HashMap();
        map.put(0, dummy);
        int count = 0;
        while (head != null) {
            count += 1;
            map.put(count, head);
            head = head.next;
        }
        ListNode preRemove = map.get(count - n);
        preRemove.next = preRemove.next.next;
        return dummy.next;
    }
}
```

2\) 不用extra space。用slow, fast node, 让fast node比slow先走N步，然后同时移动，fast 到最后时，slow便是该remove的地方

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode slow = dummy, fast = dummy;
        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }
        while (fast.next != null) {
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
```



