# 536. Construct Binary Tree from String

> You need to construct a binary tree from a string consisting of parenthesis and integers.
>
> The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.
>
> You always start to construct the **left **child node of the parent first if it exists.
>
> **Example:**  **Input:  "4\(2\(3\)\(1\)\)\(6\(5\)\)"**



1\) 用recursion的方法，先找到该node对应的value的substring，得到该node的value值。然后从第一个left括号往后找该node的left subtree对应的substring，recursive构建left subtree；同理构建right subtree。

```java
public class Solution {
    public TreeNode str2tree(String s) {
        if (s == null || s.length() == 0) return null;
        
        // Trick: 得到第一个left 括号的index位置
        int firstParen = s.indexOf("(");
        int val = firstParen == -1 ? Integer.parseInt(s) : Integer.parseInt(s.substring(0, firstParen));
        TreeNode root = new TreeNode(val);
        if (firstParen == -1) return root;
        
        int start = firstParen, leftParenCount = 0;
        for (int i = start; i < s.length(); i ++) {
            if(s.charAt(i) == '(') leftParenCount ++;
            else if (s.charAt(i) == ')') leftParenCount --;
            
            // 如果start 是从first left括号开始的话，说明这一段substring是该node的left subtree
            // 反之，该substring对应的是该node的right subtree
            if (leftParenCount == 0 && start == firstParen) {
                root.left = str2tree(s.substring(start+1, i));
                start = i + 1;
            } else if (leftParenCount == 0) root.right = str2tree(s.substring(start+1, i));
        }
        return root;
    }
}
```

2\) Iterative 的方法，使用stack。这道题目，根据题意，类似于preorder的情况。最开始看见root，然后构建left，然后是right。用一个Stack，一开始push入root，然后构建left push入Stack，如果遇见'\)'，说明该left结束，然后pop出来，此时其parent node又来到stack顶端，同理往后构建其right child。

```java
public class Solution {
    public TreeNode str2tree(String s) {
        Stack<TreeNode> stack = new Stack();
        
        // j 用于记录该node的value对应的substring的起始点
        for (int i = 0, j = i; i < s.length(); i++, j = i) {
            char c = s.charAt(i);
            if (c == ')') stack.pop(); // 该node的构建结束，可以pop出，让其parent node来到stack 顶端
            else if (c >= '0' && c <= '9' || c == '-') {
                while(i+1 < s.length() && s.charAt(i+1) >= '0' && s.charAt(i+1) <= '9') i ++;
                TreeNode cur = new TreeNode(Integer.parseInt(s.substring(j, i+1)));
                if (!stack.isEmpty()) {
                    TreeNode parent = stack.peek();
                    if (parent.left == null) parent.left = cur;
                    else parent.right = cur;
                }
                stack.push(cur);
            }
        }
        return stack.isEmpty() ? null : stack.pop();
    }
}
```



