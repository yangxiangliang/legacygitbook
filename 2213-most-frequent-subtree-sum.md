# 508. Most Frequent Subtree Sum

> Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node \(including the node itself\). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.



思路比较简单，也是类似于**Bottom-up**的思路，并且用一个map记录所有tree的sum以及其频率，一个global variable记录到目前位置最高的frequency，一个list记录出现了该frequency的sum的值。

```java
public class Solution {
    int mostFrequence = 0;
    HashMap<Integer, Integer> map = new HashMap();
    
    public int[] findFrequentTreeSum(TreeNode root) {
        List<Integer> list = new ArrayList();
        treeSum(root, list);
        int[] rst = new int[list.size()];
        for (int i = 0; i < list.size(); i ++) rst[i] = list.get(i);
        return rst;
    }
    
    public int treeSum(TreeNode root, List<Integer> list) {
        if (root == null) return 0;
        int left = treeSum(root.left, list);
        int right = treeSum(root.right, list);
        int sum = root.val + left + right;
        
        if (!map.containsKey(sum)) map.put(sum, 0);
        map.put(sum, map.get(sum) + 1);
        
        if (map.get(sum) > mostFrequence) {
            list.clear();
            list.add(sum);
        } else if (map.get(sum) == mostFrequence) list.add(sum);
        
        mostFrequence = Math.max(mostFrequence, map.get(sum));
        return sum;
    }
}
```



