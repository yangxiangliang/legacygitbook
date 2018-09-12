# 117. Populating Next Right Pointers in Each Node II

> Follow up for problem " Populating Next Right Pointers in Each Node".
>
> What if the given tree could be any binary tree? Would your previous solution still work?
>
> **Note**:
>
> * You may only use constant extra space.

1\) 容易想到的利用BFS来做 level order traversal，但是不是 constant extra space。

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    // BFS
    public void connect(TreeLinkNode root) {
        Queue<TreeLinkNode> queue = new LinkedList();
        if (root == null) return;
        queue.add(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            TreeLinkNode pre = queue.poll();
            if (pre.left != null) queue.add(pre.left);
            if (pre.right != null) queue.add(pre.right);

            for (int i = 0; i < size - 1; i++) {
                TreeLinkNode cur = queue.poll();
                pre.next = cur;
                if (cur.left != null) queue.add(cur.left);
                if (cur.right != null) queue.add(cur.right);
                pre = cur;
            }
        }
    }
}
```

2\) 不用queue来做level order traversal，用几个pointer来代替。用3个pointer，cur代表current level的node，pre代表next level的node，利用pre.next来connect cur.left或则cur.right的node。head来记录next level的第一个，因为一个level结束后，需要cur换为head。

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        TreeLinkNode cur = root; // current level node
        TreeLinkNode pre = null; // pre node in next level,用于赋值pre.next = ...
        TreeLinkNode head = null; // 记录next level的head node，cur完成一个level后，赋值 cur = head

        while(cur != null) {
            // Trick:每次开始next level的时候，pre, head必须reset为null，然后在下一个while里被重新赋值
            pre = null;
            head = null;

            while(cur != null) {
                if (cur.left != null) {
                    if (pre != null) {
                        pre.next = cur.left;
                        pre = pre.next;
                    } else pre = cur.left;

                    if (head == null) head = cur.left;
                }
                if (cur.right != null) {
                    if (pre != null) {
                        pre.next = cur.right;
                        pre = pre.next;
                    } else pre = cur.right;

                    if (head == null) head = cur.right;
                }
                cur = cur.next;
            }
            cur = head;
        }
    }
}
```

Time: O\(N\),     Space: O\(1\)

### 3）思路和上面差不多，只不过更简洁 的写法

1. 每一层用dummyHead，dummyHead.next 则是该层最开始的node。然后用root元素一直往右边 next 遍历即可。

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {

        while(root != null) {
            TreeLinkNode dummyHead = new TreeLinkNode(0);
            TreeLinkNode cur = dummyHead;
            while(root != null) {
                if(root.left != null) {
                    cur.next = root.left;
                    cur = cur.next;
                }
                if(root.right != null) {
                    cur.next = root.right;
                    cur = cur.next;
                }
                root = root.next;
            }
            root = dummyHead.next;
        }
    }
}
```



