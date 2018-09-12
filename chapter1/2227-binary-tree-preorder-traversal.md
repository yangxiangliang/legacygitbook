# 144. Binary Tree Preorder Traversal

> Given a binary tree, return the preorder traversal of its nodes' values.
>
> **Note:** Recursive solution is trivial, could you do it iteratively?

### 思路

recursive的方法很容易，主要用iterative的方法。preorder的时候，先遍历root，然后是left，再是right，由此我们先把root push到stack中，pop出的时候先压入right，再压入left，因为这样下次pop出的先是left。然后下次pop出left，同样的方法将其right, left node压入stack，直到最后stack为空。

### Code

#### 1\) Recursive

```java
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        preorder(root, rst);
        return rst;
    }
    public void preorder(TreeNode root, List<Integer> rst) {
        if (root == null) return;
        rst.add(root.val);
        preorder(root.left, rst);
        preorder(root.right, rst);
    }
}
```

或则不用helper function，用global variable

```java
public class Solution {
    List<Integer> rst = new ArrayList();

    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) return rst;
        rst.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        return rst;
    }
}
```

#### 2\) Iterative

```java
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        Stack<TreeNode> stack = new Stack();
        if (root != null) stack.push(root);
        while(!stack.empty()) {
            // in preorder, visit left node before right node, so push right node to stack before left node
            TreeNode node = stack.pop();
            rst.add(node.val);
            if (node.right != null) stack.push(node.right);
            if (node.left != null) stack.push(node.left);
        }
        return rst;
    }
}
```

#### 3\) 构造customized class \(operation, node\) 来解题

#### 1. 该种方法是比较通用的树遍历方法。可以想到对于tree的每一个node，只有可能有2种操作，一种是visit，一种是print该点的值。

#### 2. 相比较把每一个node都压入stack，可以把node和对该node对应的操作\(Visit or Print\) 也压入stack。这样每次stack.pop\(\)的时候，如果看见需要对该node print的时候， 将其值加入结果序列即可。所以这里需要构造另外一个class\(e.g. 下面code用Guide表示，表示指导\)来包括该node和对于该node的操作。

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> rst = new ArrayList();
        Stack<Guide> stack = new Stack();
        stack.push(new Guide(0, root)); // 从root开始visit起
        while(!stack.isEmpty()) {
            Guide tmp = stack.pop();
            if(tmp.node == null) continue; // defensive coding, tmp.node完全可能是null
            if(tmp.op == 1) rst.add(tmp.node.val); // 表示需要打印该node
            else {
                // preorder的意思是先print 当前node，然后visit left node，再visit right node
                // 所以这里push入的顺序是(visit, node.right), (visit, node.left), (print, node)
                stack.push(new Guide(0, tmp.node.right));
                stack.push(new Guide(0, tmp.node.left));
                stack.push(new Guide(1, tmp.node));
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



