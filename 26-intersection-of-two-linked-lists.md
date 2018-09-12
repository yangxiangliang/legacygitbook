# 160. Intersection of Two Linked Lists

> Write a program to find the node at which the intersection of two singly linked lists begins.
>
> For example, the following two linked lists:
>
> ```
> A:     a1 -> a2    ↘
>                      c1 → c2 → c3
>                    ↗            
> B:     b1 → b2 → b3
>
> begin to intersect at node c1.
> ```
>
> **Notes:**
>
> * If the two linked lists have no intersection at all, return `null`
> * The linked lists must retain their original structure after the function returns.
> * You may assume there are no cycles anywhere in the entire linked structure.
> * Your code should preferably run in O\(n\) time and use only O\(1\) memory.

这个题目的关键是怎么让2个Linked List 上面的pointer同时移动到intersection 的Node上

1）得到2个Linked List 长度的差别diff，让较长的List的Pointer从头先移动diff个。然后同时移动2的List上的pointer，直到相同的intersection node

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lengthA = getLength(headA), lengthB = getLength(headB);
        while (lengthA > lengthB) {
            headA = headA.next;
            lengthA --;
        }
        while (lengthB > lengthA) {
            headB = headB.next;
            lengthB --;
        }
        while (headA != null && headB != null) {
            if (headA == headB) return headA;
            headA = headA.next;
            headB = headB.next;
        }
        return null;
    }
    // method to getLength of Linked List
    public int getLength(ListNode head) {
        int count = 0;
        while (head != null) {
            count ++;
            head = head.next;
        }
        return count;
    }
}
```

2\) 比较tricky，最开始同时移动headA, headB, 如果2个List的长度不同，当一个pointer（比如pointerA）移动到end的时候, pointerA和pointerB 之间的差距就是ListA和ListB的长度差异\(lengthB - lengthA\)，然后将pointerA reset为另一个list的头（此为headB\)。再移动pointerA, pointerB, 此时pointerB到end的时候，将pointerB reset为headA。可以通过计算所得此时pointerA 距离end的距离是\(lengthB - \(lengthB - lengthA\)\) = lengthA。此时pointerA，pointerB到intersection node的距离应该是相同的。如果没有intersection node, pointerA 和pointerB 到各自end \(Null\) 的距离也是相同的。

```java
 /**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;
        ListNode a = headA, b = headB;
        // 如果有intersection node，a和b会相遇。如果没有intersection node, a和b会最后都等于Null
        while (a != b) {
            a = a == null? headB : a.next;
            b = b == null? headA : b.next;  
        }
        return a;
    }
}
```



