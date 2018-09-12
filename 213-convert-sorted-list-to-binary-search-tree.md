# 109. Convert Sorted List to Binary Search Tree

> Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

1\) 利用recursion, 每次找到List的中点，中点作为subtree的root，左边的List来build left tree, 右边的List来build right tree. 注意：需要将List中点前面一个node与中点断开，这样前半段List和middle node断开，不会重复build。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        if (head.next == null) return new TreeNode(head.val);
        ListNode pre = new ListNode(0);
        pre.next = head;
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            // pre is last node of first half list
            pre = pre.next;
            slow = slow.next;
            fast = fast.next.next;
        }
        // 将first half list 和 middle node断开，这样之后recursion中不会重复build
        pre.next = null;
        TreeNode root = new TreeNode(slow.val);
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(slow.next);
        return root;
    }
}
```

2\) DFS，不用维护pre node来断开first half tree 和middle node, 利用另外一个function，用head, tail 2个node来维护就可以。当遇到tail node的时候，就不继续往后走build tree了。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        return toBST(head, null);
    }
    public TreeNode toBST(ListNode head, ListNode tail) {
        // build tree does include tail, so return null here
        if (head == tail) return null;
        ListNode slow = head, fast = head;
        while(fast != tail && fast.next != tail) {
            slow = slow.next;
            fast = fast.next.next;
        }
        TreeNode root = new TreeNode(slow.val);
        root.left = toBST(head, slow);
        root.right = toBST(slow.next, tail);
        return root;
    }
}
```



