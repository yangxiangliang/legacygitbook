# 145. Binary Tree Postorder Traversal

> Given a binary tree, return the postorder traversal of its nodes' values.
>
> **Note: **Recursive solution is trivial, could you do it iteratively?

### 思路

在post order中，先visit left，然后是right, 再是root。所以一开始也想到从root开始往root.left的方向一直走，依次压入stack，此时最左边的node在stack的顶端。**\(1\)Tricky的地方在pop出元素的时候，因为post order是先visit right，再visit root，所以pop node的时候，如果该node有right node，此时需要将其right node压入stack。而且由于post order最开始visit left，所以还需要将其right node的所用left node都最开始的方法依次压入stack。\(2\) 第二个tricky的地方是遇见\(1\)情况的时候需要分辨，该node的right node是已经visit过的，还是没有visit过的。只对没有visit过的，才用\(1\)中操作。**

### Code

#### 1\) Recursive

```java
public class Solution {
    List<Integer> rst = new ArrayList();

    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) return rst;
        postorderTraversal(root.left);
        postorderTraversal(root.right);
        rst.add(root.val);
        return rst;
    }
}
```

#### 2\) Iterative

#### a.

```java
public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        Stack<TreeNode> stack = new Stack();
        // 从 root往left依次压入stack
        while(root != null) {
            stack.push(root);
            root = root.left;
        }
        while(!stack.empty()) {
            // 最开始不能pop node，先peek，观察该node是否有没有visited过的right node
            TreeNode node = stack.peek().right;
            // 检查peek node的right node是不是没有被visited过。如果已经被visit，应该是rst list中最近的一个value
            // 并且进入该if后不能pop出元素，因为此时stack顶端的node已不是原来的，可能其还有right node需要压入stack
            if (node != null && (rst.size() == 0 || node.val != rst.get(rst.size() - 1))) {
                while(node != null) {
                    stack.push(node);
                    node = node.left;
                }
            } else rst.add(stack.pop().val);
        }
        return rst;
    }
}
```

#### b.  和上面的方法大体类似，只是minor change，就是用另外一个node pointer来记录前一次从stack中pop出来的node，这样便可以得知当前peek的node的right node是不是已经被visit过了

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        Stack<TreeNode> stack = new Stack();
        while(root != null) {
            stack.push(root);
            root = root.left;
        }

        TreeNode last = null;
        while(!stack.isEmpty()) {
            TreeNode tmp = stack.peek();
            if(tmp.right == null || tmp.right == last) {
                last = stack.pop();
                rst.add(last.val);
            } else {
                stack.push(tmp.right);
                tmp = tmp.right.left;
                while(tmp != null) {
                    stack.push(tmp);
                    tmp = tmp.left;
                }
            }
        }
        return rst;
    }
}
```

#### 3\) 构造customized class \(operation, node\) 来解题。具体解释见[2.2.27 Binary Tree Preorder Traversal](/chapter1/2227-binary-tree-preorder-traversal.md)

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        Stack<Guide> stack = new Stack();
        stack.push(new Guide(0, root));
        while(!stack.isEmpty()) {
            Guide tmp = stack.pop();
            if(tmp.node == null) continue; // defensive coding
            if(tmp.op == 1) rst.add(tmp.node.val);
            else {
                // postorder的意思是先visit left node, 然后visit right node, 再print当前node
                // 所以这里push入的顺序是(print, node), (visit, node.right), (visit, node.left)
                stack.push(new Guide(1, tmp.node));
                stack.push(new Guide(0, tmp.node.right));
                stack.push(new Guide(0, tmp.node.left));
            }
        }
        return rst;
    }

    private class Guide {
        int op; // 0 means vist, 1 means print
        TreeNode node;
        public Guide(int op, TreeNode node) {
            this.op = op;
            this.node = node;
        }
    }
}
```



