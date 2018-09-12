# 297. Serialize and Deserialize Binary Tree

> Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.
>
> Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.
>
> **Note: **Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

### 思路

1. 和**2.2.10 **的方法一样，用preorder和一个queue来解决。preorder先visit root，然后left node，然后right  node。将preorder的value加上separator组成一个string。Deserialize的时候，将string  split成value放入queue。queue头一个便是root的值，然后recursion 来build  left and right subtree。
2. **\(为什么想到用queue\)** 用preorder的想法因为preorder的顺序很容易，第一个总的root点，然后先left，再right，很符合习惯，容易写code。在serialize中不用其他data structure，只用recursion来build string即可。在deserialize的时候，需要将serialize的string 按照spliter split成单独的string，放入一个data structure。因为使用的preorder，那么split后的string\[\] 第一个总是对应的root，然后才是left node, right node。第一个root create出来后，我们需要把其对应value的string从string\[\] array中去掉，用剩下的string\[\] array的第一个来create node，也就是上一个的left node，然后又将其从string\[\] array中去掉即可。如果直接把data string split后的string\[\] array放在array或则list中，不方便去掉每次create用掉的value string，而且每次都是create string\[\] array的头一个，所以想到把string\[\] array放到queue中，每次poll\(\) 出queue的头元素即可。

### Code

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



