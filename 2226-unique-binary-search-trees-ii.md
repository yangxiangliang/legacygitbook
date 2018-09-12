# 95. Unique Binary Search Trees II

> Given an integer n, generate all structurally unique **BST's **\(binary search trees\) that store values 1...n.

### 思路

还是用recursion，因为是BST，从1到n中，如果i是root，那么在下一个recursion 中用\(1, i-1\)构建所有情况的BST，return他们的root list，作为root的left subtree；同理在下一个recursion中用\(i+1, n\)构建所有的BST，return 他们的root list，作为root的right subtree。

### Code

```java
public class Solution {
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) return new ArrayList();
        return generateHelper(1, n);
    }
    public List<TreeNode> generateHelper(int start, int end) {
        List<TreeNode> list = new ArrayList();
        if (start > end) {
            // 注意这里必须要加上Null，不能直接return empty list
            // 如果这里直接return empty list，因为在下面leftNodes, rightNodes的for循环时，一边为empty list，无法进入for训话
            list.add(null);
            return list;
        }

        for (int i = start; i <= end; i ++) {
            List<TreeNode> leftNodes = generateHelper(start, i - 1);
            List<TreeNode> rightNodes = generateHelper(i + 1, end);
            // 确保能进入下面两个for循环，所以leftNodes，rightNodes不能为empty list，至少有一个Null Node
            for (TreeNode left : leftNodes) {
                for (TreeNode right: rightNodes) {
                    TreeNode root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    list.add(root);
                }
            }
        }
        return list;
    }
}
```



