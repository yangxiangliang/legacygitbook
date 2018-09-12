# 426. Convert Binary Search Tree to Sorted Doubly Linked List

> Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

### Code 1\) Recursive

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {

    Node pre = null;
    Node head = null;
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;

        treeToDoublyList(root.left);
        // 因为 pre == null，表示这是一开始，即left most node，所以assign给head值，之后head值不变
        // head的作用就是记录double linked list第一个node
        if(pre == null) head = root;
        else {
            // 当前node就是root，所以root的left指针和pre的right指针相互赋值
            root.left = pre;
            pre.right = root;
        }
        // 更新pre node 为current node
        pre = root;

        treeToDoublyList(root.right);

        // 最后pre变成double linked list最后一个node，head是开头的node，把head, pre 收尾连接起来
        head.left = pre;
        pre.right = head;
        return head;
    }
}
```

### Code 2\) Iterative

思路和上面差不多，就是用stack来inorder 遍历 tree，同时用一个pre node 记录前一个node，这样方便和current node连接起来形成doubly linked list。然后再用一个head, tail node分别记录doubly linked list的头和尾即可。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;

        Node pre = null, head = null;
        Stack<Node> stack = new Stack();

        while(root != null || !stack.isEmpty()) {
            while(root != null) {
                stack.push(root);
                root = root.left;
            }
            
            Node cur = stack.pop();
            
            // 如果 pre == null,说明是最开始left most的node，即doubly linked list的head，所以给head赋值为cur
            if(pre == null) head = cur;
            else {
                cur.left = pre;
                pre.right = cur;
            }
            pre = cur;
            
            // 重新root 赋值为 cur.right，如果cur.right不为null而且有left node，那么还要继续push to stack
            root = cur.right;
        }

        // 此时的pre就是double linkedlist的tail, 连接doubly linked list的头部和尾部        
        pre.right = head;
        head.left = pre;
        return head;
    }
}
```



