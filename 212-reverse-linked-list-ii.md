# 92. Reverse Linked List II

> Reverse a linked list from positionmton. Do it in-place and in one-pass.
>
> For example:  
> Given`1->2->3->4->5->NULL`,m= 2 andn= 4,
>
> return`1->4->3->2->5->NULL`.
>
> **Note:**  
> Givenm,nsatisfy the following condition:  
> 1 ≤m≤n≤ length of list.



1\) 较容易想到的方法， 按普通reverse list的方法reverse 其中的sublist，但是要记录sublist的开头与起点的node，以及sublist之前一个和之后一个的node，因为最后要把reversed sublist和他们连接起来

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // pre node is node before the sublist to be reversed
        ListNode pre = dummy;
        for(int i = 0; i < m-1; i++) {
            pre = pre.next;
            // head is start node in the sublist to be reversed
            head = head.next;
        }
        // start node to mark first node in sublist before reversed
        ListNode start = head;
        // end node to mark node after sublist to be reversed
        ListNode end = head;
        ListNode newHead = null;
        for(int i = 0; i <= n - m; i++) {
            end = end.next;
            ListNode next = head.next;
            head.next = newHead;
            newHead = head;
            head = next;
        }
        start.next = end;
        pre.next = newHead;
        return dummy.next;
    }
}
```



2\)  更简洁的方法，不用记录特别的点，每一次reversion，subList和总的list都是拼接在一起的，eg. 1 - &gt; 2 -&gt; 3 -&gt; 4 -&gt; 5, m=2, n=4:

1. 第一次reversion，交换2，3， 变为 1 -&gt; 3 -&gt; 2 -&gt;4 -&gt; 5
2. 第二次reversion，交换3，4，变为 1 -&gt; 4 -&gt; 3-&gt; 2 -&gt; 5

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // pre node is node before the sublist to be reversed
        ListNode pre = dummy;
        for(int i = 0; i < m-1; i++) {
            pre = pre.next;
        }
        // start node to mark first node in sublist before reversed
        ListNode start = pre.next;
        // then node to mark node need to be reversed 
        ListNode then = start.next;
        for(int i = 0; i < n - m; i++) {
            // Tricky part, draw a graph for easy understanding
            start.next = then.next;
            then.next = pre.next;
            pre.next = then;
            then = start.next;
        }
        
        // first reversing : dummy->1 - 3 - 2 - 4 - 5; pre = 1, start = 2, then = 4
        // second reversing: dummy->1 - 4 - 3 - 2 - 5; pre = 1, start = 2, then = 5 (finish)
        return dummy.next;
    }
}
```











