# 96. Unique Binary Search Trees

> Given n, how many structurally unique **BST's **\(binary search trees\) that store values 1...n ?
>
> **For example, **
>
> Given n = 3, there are a total of 5 unique BST's.



此题因为value为n的结果可以用到value为\(n-1\)的结果，典型的可以用dynamic programming。假设一个tree store values from 1...n，假设此时root的value取k，由于是BST，所有小于K的在root左边，大于K的在root右边。左边的subtree相当于tree store values from 1....k, 右边的subtree相当于tree store values from k+1, ...n。Dynamic 递推公式:

_F\(n\) = F\(0\)\*F\(n-1\) + F\(1\)\*F\(n-2\) + F\(k-1\)\*F\(n-k\)+....+F\(n-1\)\*F_\(0\)

```java
public class Solution {
    public int numTrees(int n) {
        int[] rst = new int[n+1];
        rst[0] = 1;
        rst[1] = 1;
        for (int i = 2; i <= n; i ++) {
            for (int j = 1; j <= i; j ++) {
                rst[i] += rst[j-1] * rst[i-j];
            }
        }
        return rst[n];
    }
}
```



