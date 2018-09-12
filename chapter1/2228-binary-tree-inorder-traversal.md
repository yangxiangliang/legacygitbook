# 94. Binary Tree Inorder Traversal

> Given a binary tree, return the inorder traversal of its nodes' values.
>
> Recursive solution is trivial, could you do it iteratively?

### 思路

对于Iterative的方法，同样是用stack。用于这里是inorder，需要先visit left node，然后是root node，再是right node。一开始从root压入stack，然后一直往left走，把left node都压入stack，这样最left node在stack的顶部，应该pop该node。要注意的是，pop出的node如果有right node，需要将right node 压入stack，而且如果该right node还有left node，需要将其left node都要依次压入stack，这样下一次pop出的才是right node或则right node的最left的node。

### Code

#### 1\) Recursive

```java
public class Solution {
    List<Integer> rst = new ArrayList();

    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) return rst;
        inorderTraversal(root.left);
        rst.add(root.val);
        inorderTraversal(root.right);
        return rst;
    }
}
```

#### 2\) Iterative

```java
public class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        Stack<TreeNode> stack = new Stack();
        // push left nodes of root to stack
        while(root != null) {
            stack.push(root);
            root = root.left;
        }
        while(!stack.empty()) {
            TreeNode node = stack.pop();
            rst.add(node.val);
            // push node.right and left nodes of node.right to stack if exist
            node = node.right;
            while(node != null) {
                stack.push(node);
                node = node.left;
            }
        }
        return rst;
    }
}
```

#### 3\) 构造customized class \(operation, node\) 来解题。具体解释见 [2.2.27 Binary Tree Preorder Traversal](/chapter1/2227-binary-tree-preorder-traversal.md)

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        Stack<Guide> stack = new Stack();
        stack.push(new Guide(0, root));
        while(!stack.isEmpty()) {
            Guide tmp = stack.pop();
            if(tmp.node == null) continue; // defensive coding
            if(tmp.op == 1) rst.add(tmp.node.val);
            else {
                // inorder的意思是先visit left node，然后print 当前node，再visit right node
                // 所以这里push入的顺序是(visit, node.right), (print, node), (visit, node.left)
                stack.push(new Guide(0, tmp.node.right));
                stack.push(new Guide(1, tmp.node));
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



