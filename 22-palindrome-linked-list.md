# 234. Palindrome Linked List

> Given a singly linked list, determine if it is a palindrome.
>
> **Follow up:**  
> Could you do it in O\(n\) time and O\(1\) space?

* 首先这个题目需要用到 reverse linked list function
* 需要用快慢指针找到middle node, 然后reverse后面一半，和前一半相比较

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
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode newHead = reverse(slow);
        while(head != null && newHead != null) {
            if (head.val != newHead.val) {
                return false;
            }
            head = head.next;
            newHead = newHead.next;
        }
        return true;
    }
    // 注意熟练写出 reverse linked list 的函数
    public ListNode reverse(ListNode head) {
        if (head == null) return null;
        // 注意此时pre node的用法，初始值为Null
        ListNode pre = null;
        while(head != null) {
            ListNode next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
}
```



