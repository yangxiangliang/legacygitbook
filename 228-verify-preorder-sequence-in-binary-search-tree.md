# 255. Verify Preorder Sequence in Binary Search Tree

> Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.
>
> You may assume each number in the sequence is unique.
>
> **Follow up:**
>
> Could you do it using only constant space complexity?



这个题目类似用Stack iterative 的方法做BST的preorder traversal，比如BST \[5，3，7，2，4，null, 8\]，其preorder traversal是\[5，3，2，4，7，8\]。从preorder中取值顺序取值i，如果stack是空或则i &lt; stack.peek\(\)，说明BST一直往left tree走，把i一直压入stack。当i &gt; stack.peek\(\)时，说明已经来到了某个node的right tree，那么此时之后的i值都需要有一个low bound，可以得到之后i值必须大于该node的值，所以将stack.pop\(\)直到stack.peek\(\)大于i，并且每次pop出的都是之后所有遍历到的值的下限值。以上两种情况，最后都需要push 入i 值。

1）使用Stack, not constant space complexity

```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        Stack<Integer> stack = new Stack();
        int low = Integer.MIN_VALUE;
        
        for (int i : preorder) {
            if (i < low) return false;
            while(!stack.isEmpty() && i > stack.peek()) {
                low = stack.pop();
            }
            stack.push(i);
        }
        return true;
    }
}
```

2\) 使用constant space complexity，思路是一样的，但是不用constant，reuse input array。用index i 遍历input array，当发现preorder\[i\] &gt; preorder\[i-1\]的时候，往前遍历找low bound。此时有一个trick需要注意：因为这里reuse input array，不像上面stack可以pop出left tree的元素\(或则 理解为已经用来update 过low bound的node\)。比如 array \[5, 3, 2, 4 .......\], 到4的时候往前遍历找Low bound，之后从4往后走，可能往前再次遍历的时候又会经过“3，2”，但是low bound理论上应该越来越大，所以这里必须要Math.max\(low\_bound, preorder\[j\]\) 来处理。或则把已经遍历过的部分\(比如"3, 2"\)在从4往前找low bound的时候，把\[3, 2\]改成4来避免重复。

```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        int low = Integer.MIN_VALUE;
        for (int i = 0; i < preorder.length; i ++) {
            if (preorder[i] < low) return false;
            
            if (i != 0 && preorder[i] > preorder[i-1]) {
                for (int j = i-1; j >=0; j --) {
                    if (preorder[j] < preorder[i]) {
                        low = Math.max(preorder[j], low); // 不能 只有 low = preorder[j], 这样会使Low 变得更小。
                        // preorder[j] = preorder[i];  // 如果上一行为 low = preorder[j], 这一行加一个preorder[j] = preorder[i]
                    }
                    else break;
                }
            }
        }
        return true;
    }
}
```



