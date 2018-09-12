# 106. Construct Binary Tree from Inorder and Postorder Traversal

> Given inorder and postorder traversal of a tree, construct the binary tree.
>
> **Note:**
>
> You may assume that duplicates do not exist in the tree.

注意这里不是binary search tree的traversal，而是普通binary tree的traversal。对于int\[\] inorder，int\[\] postorder，由于是postorder traversal，所以最后visit root，所以postorder的最后一个元素是root的value。然后找到该value在inorder数组中的index，inorder是先left tree，然后root，再right tree，所以该index左边全是left subtree，index右边全是right subtree。通过以上两端subarray的长度，又可以在postorder里找到对应的subarray。然后这些subarray的末尾又是该subtree的root，依次类推recursion来构建tree。

1\)  Time: O\(N^2\)，因为要access 每个node，而且search也是O\(N\)

```java
public class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        return buildHelper(inorder, postorder, 0, inorder.length-1, 0, postorder.length-1);
    }
    
    public TreeNode buildHelper(int[] inorder, int[] postorder, int istart, int iend, int pstart, int pend) {
        if (istart > iend) return null;
        TreeNode root = new TreeNode(postorder[pend]);
        int rootIndex = search(inorder, postorder[pend], istart, iend);
        // left subtree的长度是rootIndex-1-istart, 加上pstart，得到left subtree的pend值
        root.left = buildHelper(inorder, postorder, istart, rootIndex-1, pstart, pstart + rootIndex-1-istart);
        // right subtree的长度是iend-rootIndex-1, 根据该长度和(pend-1)来得到该right subtree的pstart index值
        root.right = buildHelper(inorder, postorder, rootIndex+1, iend, pend-iend+rootIndex, pend-1);
        return root;
    }
    
    public int search(int[] inorder, int target, int start, int end) {
        for (int i = start; i <= end; i ++) {
            if (inorder[i] == target) return i;
        }
        return -1;
    }
}
```

2\) 思路同上，每次都在inorder array里search value特别inefficient，所以可以用一个hashMap\(此题没有duplicates\)记录inorder array中value和index的mapping。 Time: O\(N\), Space: O\(N\)

```java
public class Solution {
    HashMap<Integer,Integer> map=new HashMap();
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for(int i=0;i<inorder.length;i++)
            map.put(inorder[i],i);
        return helper(inorder,postorder,0,inorder.length-1,0,postorder.length-1);
    }
    private TreeNode helper(int[] inorder,int[] postorder, int instart,int inend,int poststart,int postend){
        if(instart>inend)
           return null;
        int value=postorder[postend];
        TreeNode root=new TreeNode(value);
        int index=map.get(value);
        root.left=helper(inorder,postorder,instart,index-1,poststart,poststart+index-1-instart);
        root.right=helper(inorder,postorder,index+1,inend,postend-1-inend+index+1,postend-1);
        return root;
    }
}
```



