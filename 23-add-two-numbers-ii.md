# 445. Add Two Numbers II

> You are given two **non-empty **linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
>
> You may assume the two numbers do not contain any leading zero, except the number 0 itself.
>
> **Follow up:**  
> What if you cannot modify the input lists? In other words, reversing the lists is not allowed.
>
> **Example:**
>
> **Input**:  \(7 -&gt; 2 - &gt; 4 -&gt; 3\) + \(5 -&gt; 6 -&gt; 4\)
>
> **Output**: 7 -&gt; 8 -&gt; 0 -&gt; 7

能想到2个方法

1. Reverse input lists, 然后一个一个加起来，再reverse一次结果
2. 如果不reverse list，可以用Stack解决。因为Stack是LIFO，List压入Stack后，最先pop出的是个位

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 // If reversing the lists is not allowed, we can use Stack to solve it
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<Integer> stack1 = new Stack();
        Stack<Integer> stack2 = new Stack();
        while (l1 != null) {
            stack1.push(l1.val);
            l1 = l1.next;
        }
        while(l2 != null) {
            stack2.push(l2.val);
            l2 = l2.next;
        }
        ListNode head = new ListNode(0);
        ListNode tail = null;
        int carry = 0;
        while (!stack1.empty() || !stack2.empty()) {
            int a=0, b=0;
            if (!stack1.empty()) {
                a = stack1.pop();
            }
            if (!stack2.empty()) {
                b = stack2.pop();
            }
            int total = carry + a + b;
            ListNode newNode = new ListNode(total%10);
            carry = total / 10;
            newNode.next = tail;
            tail = newNode;
        }
        head.val = carry;
        head.next = tail;
        return head.val == 0 ? head.next : head;
    }
}
```



