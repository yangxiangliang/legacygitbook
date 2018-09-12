# 23. Merge K Sorted Lists

> Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

1\) 利用recursion，将所有Lists分成two half, 然后先分别merge两个half生成2个list，再merge这两个List

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
    public ListNode mergeKLists(ListNode[] lists) {
        int size = lists.length;
        if (size == 0) return null;
        if (size == 1) return lists[0];
        ListNode l1 = mergeKLists(Arrays.copyOfRange(lists, 0, size/2)); // using extra space
        ListNode l2 = mergeKLists(Arrays.copyOfRange(lists, size/2, size)); // using extra space
        return merge(l1, l2);
    }

    public ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while(l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            } else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = (l1 == null) ? l2 : l1;
        return dummy.next;
    }
}
```

2\) 思路和上面差不多，但是不用extra space来copy lists, 用一个sub function来pass 2个start和end index，来限制需要merge的Lists的范围。

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
    public ListNode mergeKLists(ListNode[] lists) {
        return mergeKHelper(lists, 0, lists.length);
    }

    public ListNode mergeKHelper(ListNode[] lists, int start, int end) {
        if (start == end) return null;
        if (end - start == 1) return lists[start];
        int middle = start + (end-start) / 2;
        return mergeTwoLists(mergeKHelper(lists, start, middle), mergeKHelper(lists, middle, end));
    }

    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while(l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            } else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = (l1 == null) ? l2 : l1;
        return dummy.next;
    }
}
```

3\) 使用一个Min Heap，size是所有Lists的个数，初始化是把每个list的head放进去，每次heap.poll\(\)一个node出来连接到最后return的List上, 然后把node.next又重新add进heap，知道最后heap 变成empty。

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) return null;
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        Queue<ListNode> heap = new PriorityQueue<ListNode>(lists.length, new Comparator<ListNode>() {
            public int compare(ListNode l1, ListNode l2) {
                return l1.val - l2.val;
            }
        });
        for(ListNode head: lists) {
            if (head != null) heap.add(head);
        }
        while(heap.size() > 0) {
            ListNode tmp = heap.poll();
            cur.next = tmp;
            tmp = tmp.next;
            if (tmp != null) heap.add(tmp);
            cur = cur.next;
        }
        return dummy.next;
    }
}
```

4\) 和前面 \(1\), \(2\) 的思路差不多，也是merge sort来做。但是不用recursion，这样没有recursion 函数产生的stack函数占用空间，可以做到 space complexity O\(1\), time complexity 还是 O\(N \*log\(K\)\)。N是一共K个list的所有node的个数，K是list的数量。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        int size = lists.length;
        int interval = 1;
        
        while(interval < size) {
            // 画个lists的图形，容易理解这里for 循环的思路
            // e.g lists[1, 2, 3, 4, 5]：先merge(1, 2), (3, 4)； 再merge(1, 3)；最后merge(1, 5)
            for(int i = 0; i + interval < size; i += 2 * interval) {
                lists[i] = mergeTwoLists(lists[i], lists[i + interval]);
            }
            interval = 2 * interval;
        }
        return size > 0 ? lists[0] : null;
    }
    
    private ListNode mergeTwoLists(ListNode a, ListNode b) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        
        while(a != null && b != null) {
            if(a.val < b.val) {
                cur.next = a;
                a = a.next;
            } else {
                cur.next = b;
                b = b.next;
            }
            cur = cur.next;
        }
        cur.next = a == null ? b : a;
        return dummy.next;
    }
}
```



