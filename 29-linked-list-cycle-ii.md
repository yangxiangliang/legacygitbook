# 142. Linked List Cycle II

> Given a linked list, return the node where the cycle begins. If there is no cycle, return`null`.
>
> **Note:**Do not modify the linked list.
>
> **Follow up**:  
> Can you solve it without using extra space?

1）容易的方法，使用extra space, 第一个遇见第二次的node便是cycle的起点

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        HashSet<ListNode> set = new HashSet();
        while(head != null) {
            if (set.contains(head)) return head;
            else set.add(head);
            head = head.next;
        }
        return null;
    }
}
```

2\) 使用快慢指针，slow， fast pointers。 假设List的长度为L\(不考虑cycle loop，L相当于所有节点个数和\)，cycle长度为C。fast追上slow必须是比slow多走一个cycle，即C。一次，fast比slow多走1，所以追上时，多走了C/1个单位时间。此时他们距离head为C，head距离cycle起点为L-C。相遇点 距离cycle起点也为L-C（画图更容易理解）。然后将fast pointer reset到head，和slow同步调，便可以同时到达cycle start point.

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            // 不要把this line放到while循环第一个，因为slow, fast初始化就是相等的
            if (slow == fast) break;
        }
        if (fast == null || fast.next == null) return null;
        fast = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```



