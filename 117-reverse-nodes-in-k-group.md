# 25. Reverse Nodes in k-Group

> Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.
>
> k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k
>
> then left-out nodes in the end should remain as it is.
>
> You may not alter the values in the nodes, only nodes itself may be changed.
>
> Only constant memory is allowed.
>
> For example,
>
> Given this linked list: 1-&gt;2-&gt;3-&gt;4-&gt;5
>
> For k = 2, you should return: 2-&gt;1-&gt;4-&gt;3-&gt;5
>
> For k = 3, you should return: 3-&gt;2-&gt;1-&gt;4-&gt;5

* 首先check从当前node往下走，是否还有多于K个node。如果有，则继续reverse，不然便结束。
* reverseNextK node的时候需要return reverse这段List的tail，需要作为下一段reverse list的pre node。



1\) 不使用recursion

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
    public ListNode reverseKGroup(ListNode head, int k) {
        // special case
        if (head == null || head.next == null || k == 1) return head;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        while(hasMoreThanK(pre, k)) {
            ListNode tail = reverseNextK(pre, k);
            pre = tail;
        }
        return dummy.next;
    }

    public ListNode reverseNextK(ListNode pre, int k) {
        ListNode cur = pre.next;
        // 这里比较绕，比如reverse: pre->1-> 2- >3
        // step 1变成: pre ->2->1->3
        // step 2 变成: pre ->3->2->1
        for (int i = 0; i < k-1; i ++) {
            ListNode curNext = cur.next;
            ListNode preNext = pre.next;
            pre.next = curNext;
            cur.next = cur.next.next;
            pre.next.next = preNext;
        }
        return cur;
    }

    public boolean hasMoreThanK(ListNode head, int k) {
        if (head == null) return false;
        for (int i = 0; i < k; i++) {
            head = head.next;
            if (head == null) return false;
        }
        return true;
    }
}
```

2）这道题可以使用recursion，从当前node找到第\(k+1\)个node, 用recursion reverse从\(k+1\)后的List, 然后reverse current k nodes group。

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
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode cur = head;
        int count = 0;
        // to find if there are next k nodes
        while(cur != null && count < k) {
            cur = cur.next;
            count ++;
        }
        if (count == k) {
            cur = reverseKGroup(cur, k);
            while(count-- > 0) {
                ListNode next = head.next; // 记录此时head.next，其即将变成current k nodes group的first node
                head.next = cur; // append head 到reversed list
                cur = head; // update reversed list的first node
                head = next; // update current k nodes group中还没有reversed的first node
            }
            //此处不return head, 因为最后head已经变成下一段reversed list的first node了
            return cur;
        }
        return head;
    }
}
```



