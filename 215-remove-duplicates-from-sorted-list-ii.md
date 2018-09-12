# 82. Remove Duplicates from Sorted List II

> Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.
>
> For example,
>
> Given 1-&gt;2-&gt;3-&gt;3-&gt;4-&gt;4-&gt;5, return 1-&gt;2-&gt;5
>
> Given 1-&gt;1-&gt;1-&gt;2-&gt;3. return 2-&gt;3

用2个指针，pre 和 cur\(head\) node, 如果cur 和 cur.next 的value相等，cur往后移动。通过pre, cur是不是间隔为1来判断是否有duplicate，如果没有duplicate，cur应该就是pre的下一个。

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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        while(head != null) {
            // head 跳过 duplicate的List
            while(head.next != null && head.next.val == head.val) {
                head = head.next;
            }
            // 关键，如果 pre.next != head(current node), 中间有duplicate，pre.next = head.next后不后移pre
            // 有可能 head.next 也是一段duplicate的开始。
            if (pre.next != head) {
                pre.next = head.next;
            } else pre = pre.next;
            head = head.next;
        }
        return dummy.next;
    }
}
```



