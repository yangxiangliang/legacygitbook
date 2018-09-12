# 230. Kth Smallest Element in a BST

> Given a binary search tree, write a function **kthSmallest **to find the **kth **smallest element in it.
>
> **Note:**
>
> You may assume k is always valid, 1 &lt;= k &lt;= BST's total elements.
>
> **Follow up:**
>
> What if the BST is modified \(insert/delete operations\) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

### 思路

由于是BST，而且找Kth smallest value，联想到BST的inorder traversal是递增数列，走到第K个node的时候，其value便是结果。所以用一个helper function做inorder traversal，然后用一个global variable来记录遍历到的current node的排序，如果到第K个，该值便是结果。同样，可以用一个global variable来记录最后结果。

**注意：下面 global variable "count++" 的位置，易错！！易错！！易错！！**

### Code 1\)

```java
public class Solution {
    int count = 0;
    int rst = 0;

    public int kthSmallest(TreeNode root, int k) {
        inorder(root, k);
        return rst;
    }

    public void inorder(TreeNode node, int k) {
        if (node == null) return;
        inorder(node.left, k);
        // 注意，count ++ 不能在 if{}的后面，因为满足if条件的时候，直接return，那么还不能执行 count ++语句。
        // return到上一层recursion中时，count值没变，还是满足 count == k的条件，又会重新overwrite
        count ++;
        if (count == k) {
            rst = node.val;
            return;
        }
        inorder(node.right, k);
    }
}
```

### Code 2\)

思路一样，另外一种写法不用把结果rst作为global variable。那么这样就需要inorder函数return回所需结果。

**Trick的地方，**因为如果让inorder函数return int的话，很难区分该int是结果，还是找不到结果时候return的default 值，因为node的value可以是任何值，不容易把node的value值和default值区分开来。**所以这里才去直接return结果对应的那个node即可，如果在subtree中没有找到node，return null即可。**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int index = 0;
    public int kthSmallest(TreeNode root, int k) {
        return inorder(root, k).val;
    }

    private TreeNode inorder(TreeNode node, int k) {
        if(node == null) return null;
        TreeNode left = inorder(node.left, k);
        if(left != null) return left; // left == null, 说明node.left subtree中没有找到target node
        index++;
        if(index == k) return node;
        TreeNode right = inorder(node.right, k);
        return right; // 无论right 是 null与否，return 即可
    }
}
```

### Code 3\)

不用recursion的方法。就用iterative的方法，思路就是stack 装 node，用stack和iterative的方法对tree做inorder的便利。到了Kth smallest integer的时候，直接返回其值即可。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack();
        TreeNode node = root;
        stack.push(node);
        // 压入所有left的nodes
        while(node.left != null) {
            stack.push(node.left);
            node = node.left;
        }
        
        while(!stack.isEmpty()) {
            if(k == 1) return stack.peek().val;
            k --;
            node = stack.pop();
            
            if(node.right != null) {
                stack.push(node.right);
                node = node.right;
                while(node.left != null) {
                    stack.push(node.left);
                    node = node.left;
                }
            }
        }
        return -1;
    }
}
```



