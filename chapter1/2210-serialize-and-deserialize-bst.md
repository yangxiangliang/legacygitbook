# 297. Serialize and Deserialize BST

> Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.
>
> Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.
>
> **The encoded string should be as compact as possible.**
>
> **Note:**Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

1\) 最容易想到的就是BFS的方法，level by level traversal，然后记录各个点的value，必要的时候用null 代替。

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
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) return "";
        StringBuffer rst = new StringBuffer();
        Queue<TreeNode> queue = new LinkedList();
        queue.add(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0;i < size; i++) {
                TreeNode node = queue.poll();
                if (node == null) rst.append("#"+"null");
                else if (rst.length() == 0) rst.append(""+ node.val);
                else rst.append("#" + node.val);
                if (node != null) {
                    queue.add(node.left);
                    queue.add(node.right);
                }
            }
        }
        return rst.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.length() == 0) return null;
        String[] values = data.split("#");
        int index = 0;
        Queue<TreeNode> queue = new LinkedList();
        TreeNode root = new TreeNode(Integer.parseInt(values[index++]));
        queue.add(root);
        while(index < values.length - 1) {
            TreeNode node = queue.poll();
            // construct lefe node
            if(!values[index].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(values[index]));
                queue.add(node.left);
            }
            index++;
            // construct right node
            if(!values[index].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(values[index]));
                queue.add(node.right);
            }
            index ++;
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

2\)  用preorder和queue来解决。preorder先visit root，然后left node，然后right  node。将preorder的value加上separator组成一个string。Deserialize的时候，将string  split成value放入queue。queue头一个便是root的值，然后recursion 来build  left and right subtree。

```java
public class Codec {
    private static final String spliter = ",";
    private static final String NULL = "X";

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder rst = new StringBuilder();
        buildString(root, rst);
        return rst.toString();
    }
    public void buildString(TreeNode node, StringBuilder rst) {
        if (node == null) rst.append(NULL+spliter);
        else rst.append(node.val + spliter);
        if (node != null) {
            buildString(node.left, rst);
            buildString(node.right, rst);
        }
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        Queue<String> queue = new LinkedList();
        for(String tmp : data.split(",")) queue.add(tmp);
        return buildTree(queue);
    }
    public TreeNode buildTree(Queue<String> queue) {
        if (queue.isEmpty()) return null;
        String tmp = queue.poll();
        if (tmp.equals(NULL)) return null;
        TreeNode root = new TreeNode(Integer.parseInt(tmp));
        root.left = buildTree(queue);
        root.right = buildTree(queue);
        return root;
    }
}
```



