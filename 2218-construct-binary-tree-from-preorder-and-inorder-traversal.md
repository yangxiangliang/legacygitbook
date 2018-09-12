# 105. Construct Binary Tree from Preorder and Inorder Traversal

> Given preorder and inorder traversal of a tree, construct the binary tree.
>
> **Note:**
>
> You may assume that duplicates do not exist in the tree.



思路和前一题**2.2.17**类似，只不过这里是preorder和inorder traversal，对于preorder，开头的元素是root value。其他分析类似。

```java
public class Solution {
    HashMap<Integer,Integer> map=new HashMap();
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i=0;i<inorder.length;i++)
            map.put(inorder[i],i);
        return helper(inorder, preorder, 0, inorder.length-1, 0, preorder.length-1);
    }
    
    private TreeNode helper(int[] inorder,int[] preorder, int istart,int iend,int pstart,int pend){
        if(istart > iend) return null;
        int value = preorder[pstart];
        TreeNode root=new TreeNode(value);
        int index = map.get(value);
        root.left = helper(inorder, preorder, istart, index-1, pstart+1, pstart+index-istart);
        root.right = helper(inorder, preorder, index+1, iend, pend-iend+index+1, pend);
        return root;
    }
}
```



