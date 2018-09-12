# 147. Insertion Sort List

> Sort a linked list using insertion sort.



此题有坑，思路是创见一个dummy node, 作为return的sorted list的开头一个，自然取值为Integer.MIN\_VALUE。

注意：最开始，不能将dummy node和head连起来，比如dummy.next = head。这样的话，最后会形成cycle, exceed memory limit. 思路应该是 将dummy node作为一个 **全新的Linked List ， **往里面添加Node，切记一开始不能将dummy node与original list 连接起来



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
    public ListNode insertionSortList(ListNode head) {
        ListNode dummy = new ListNode (Integer.MIN_VALUE);
        ListNode cur = head; // 千万不能 dummy.next = head; dummy node的List是一个全新List,后面直接往里insert node
        while (cur != null) {
            // pre 是用来遍历return的sorted list(即dummy node开头的List)，找insert position的
            ListNode pre = dummy;
            ListNode next = cur.next;
            // insert position 是 pre 与 pre.next 之间
            while (pre.next != null && cur.val > pre.next.val) {
                pre = pre.next;
            }
            cur.next = pre.next;
            pre.next = cur;
            cur = next;
        }
        return dummy.next;
    }
}
```



