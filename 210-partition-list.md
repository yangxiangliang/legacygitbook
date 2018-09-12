# 86. Partition List

> Given a linked list and a valuex, partition it such that all nodes less thanxcome before nodes greater than or equal tox.
>
> You should preserve the original relative order of the nodes in each of the two partitions.
>
> For example,  
> Given`1->4->3->2->5->2`and x= 3,  
> return`1->2->2->4->3->5`.

将结果看成两个List合并起来的，一个List包含的node比x小，另一个List包含的node不比x小。用两个dummy head 分别create 2个List，最后将他们连起来。

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
    public ListNode partition(ListNode head, int x) {
        ListNode leftDummy = new ListNode(0), rightDummy = new ListNode(0);
        ListNode leftHead = leftDummy, rightHead = rightDummy;
        while (head != null) {
            if (head.val < x) {
                leftHead.next = head;
                leftHead = leftHead.next;
            } else {
                rightHead.next = head;
                rightHead = rightHead.next;
            }
            head = head.next;
        }
        // 坑：不然rightHead.next 可能还连着其他node
        rightHead.next = null;
        leftHead.next = rightDummy.next;
        return leftDummy.next;
    }
}
```



