# 369. Plus One Linked List

> Given a non-negative integer represented as **non-empty **a singly linked list of digits, plus one to the integer.
>
> You may assume the integer do not contain any leading zero, except the number 0 itself.
>
> The digits are stored such that the most significant digit is at the head of the list.
>
> **Example**:
>
> Input: 1 -&gt; 2 -&gt; 3
>
> Output: 1 -&gt; 2 -&gt; 4

比较简洁的方法用 `Recursive`

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
    public ListNode plusOne(ListNode head) {
        ListNode newHead = new ListNode(plusOneHelper(head));
        newHead.next = head;
        return newHead.val == 0 ? newHead.next : newHead;
    }
    public int plusOneHelper(ListNode head) {
        if (head == null) return 1; // 不是return 0, 因为最后一位要Plus One. 类推Plus N,可以类似解法
        int carry = plusOneHelper(head.next);
        int total = head.val + carry;
        head.val = total % 10;
        return total / 10;
    }
}
```



