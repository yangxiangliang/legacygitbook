# 545. Boundary of Binary Tree

> Given a binary tree, return the values of its boundary in **anti-clockwise **direction starting from root. Boundary includes left boundary, leaves, and right boundary in order without duplicate nodes.
>
> **Left boundary **is defined as the path from root to the **left-most **node. **Right boundary **is defined as the path from root to the **right-most **node. If the root doesn't have left subtree or right subtree, then the root itself is left boundary or right boundary. Note this definition only applies to the input binary tree, and not applies to any subtrees.
>
> The **left-most **node is defined as a **leaf **node you could reach when you always firstly travel to the left subtree if exists. If not, travel to the right subtree. Repeat until you reach a leaf node.
>
> The **right-most **node is also defined by the same way with left and right exchanged.



根据题目的意思来整理思路，用一个helper function 以及List&lt;List&lt;Integer&gt;&gt; rst 记录每一条从root到leaf的路径，当然按照先left，再right的顺序来，因为这样可以满足anti-clockwise的要求。需要注意的是rst中index = 0的list是left boundary，index = rst.size\(\) - 1 的list是right boundary，最后需要reverse加入结果中。其他的只用加list的最后一个元素，即leaf。然后special case是，如果root.left == null，那么left boundary 是root.val，需要额外加入；如果root.right == null，那么right boundary 是root.val。而且root.val只能加入结果中一次，避免duplicates。



1\) 自己写的solution，有点verbose

```java
public class Solution {
    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
        List<List<Integer>> rst = new ArrayList();
        if (root == null) return new ArrayList();
        
        boundaryHelper(root, rst, new ArrayList());
        List<Integer> rootList = new ArrayList();
        rootList.add(root.val);
        // add left boundary, which is only root.val 
        if (root.left == null) rst.add(0, rootList);
        // add right boundary, which is only root.val
        else if (root.right == null) rst.add(rootList);
        
        List<Integer> list = new ArrayList();
        int size = rst.size();
        for (int i = 0; i < size; i ++) {
            if (i == 0) { 
                list.addAll(rst.get(0));
            } else if (i == size -1) {
                List<Integer> lastList = rst.get(size - 1);
                // Do not add last element, because it is root.val, avoid duplicates
                for (int j = lastList.size() - 1; j > 0; j --) {
                    list.add(lastList.get(j));
                }
            } else {
                List<Integer> tmp = rst.get(i);
                // Only add leaf here
                list.add(tmp.get(tmp.size() - 1));
            }
        }
        return list;
    }
    
    public void boundaryHelper(TreeNode root, List<List<Integer>> rst, List<Integer> list) {
        list.add(root.val);
        if (root.left == null && root.right == null) {
            rst.add(new ArrayList(list));
            // must remove last element here
            list.remove(list.size()-1);
            return;
        }
        
        if (root.left != null) boundaryHelper(root.left, rst, list);
        if (root.right != null) boundaryHelper(root.right, rst, list);
        // Do not forget remove last element which was added in this DFS level
        list.remove(list.size()-1);
    }
}
```

2\) 别人的方法，基本思路类似，但是不用List&lt;List&lt;Integer&gt;&gt; rst记录每一条从root 到leaf的path。

```java
List<Integer> nodes = new ArrayList<>(1000);
public List<Integer> boundaryOfBinaryTree(TreeNode root) {
    
    if(root == null) return nodes;

    nodes.add(root.val);
    leftBoundary(root.left);
    leaves(root.left);
    leaves(root.right);
    rightBoundary(root.right);
    
    return nodes;
}
public void leftBoundary(TreeNode root) {
    // left boundary的时候不加入leaf，因为所有leaf都会在 function leaves()中加入结果
    if(root == null || (root.left == null && root.right == null)) return;
    nodes.add(root.val);
    if(root.left == null) leftBoundary(root.right);
    else leftBoundary(root.left);
}
public void rightBoundary(TreeNode root) {
    // right boundary的时候不加入leaf，因为所有leaf都会在 function leaves()中加入结果
    if(root == null || (root.right == null && root.left == null)) return;
    if(root.right == null)rightBoundary(root.left);
    else rightBoundary(root.right);
    nodes.add(root.val); // add after child visit(reverse)
}
public void leaves(TreeNode root) {
    if(root == null) return;
    if(root.left == null && root.right == null) {
        nodes.add(root.val);
        return;
    }
    leaves(root.left);
    leaves(root.right);
}
```



