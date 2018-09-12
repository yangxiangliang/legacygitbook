# 298. Binary Tree Longest Consecutive Sequence

> Given a binary tree, find the length of the longest consecutive sequence path.
>
> The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child \(cannot be the reverse\).
>
> **Example 1:**
>
> ```
> Input:
>
>    1
>     \
>      3
>     / \
>    2   4
>         \
>          5
>
> Output: 3
>
> Explanation: Longest consecutive sequence path is 3-4-5, so return 3.
> ```
>
> **Example 2:**
>
> ```
> Input:
>
>    2
>     \
>      3
>     / 
>    2    
>   / 
>  1
>
> Output: 2 
>
> Explanation: Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.
> ```

### 思路

1. 思路比较直接，就是“演绎法”。从root开始往下走，如果找到2个连续递增的node值，那么counter + 1，然后需要一个global变量来记录最后结果的最大值。每次counter变化后，则update max 值即可。
2. 如果当前node与其parent node不是递增 1 的关系，那么需要reset counter的值为1。因为每个node都可以被看做长度为1的path。

3. 为了知道current node是不是在一条consecutive sequence上面的，需要helper function中parse 前一个parent node的value，来和当前node 的value作比较。

### 复杂度

Time: O\(N\)，N 是tree中所有node的个数。 因为需要遍历所有的nodes

### Code

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
public class Solution {
    int max = 1;
    
    public int longestConsecutive(TreeNode root) {
        if (root == null) return 0;
        countHelper(root, 0, root.val - 1);
        return max;
    }
    
    public void countHelper(TreeNode node, int count, int preValue) {
        if (node == null) return;
        if (node.val == preValue + 1) {
            count += 1;
            max = Math.max(max, count);
        } else {
            count = 1;
        }
        countHelper(node.left, count, node.val);
        countHelper(node.right, count, node.val);
    }
}
```



