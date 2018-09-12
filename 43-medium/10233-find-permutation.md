# 484. Find Permutation

> By now, you are given a **secret signature **consisting of character 'D' and 'I'. 'D' represents a decreasing relationship between two numbers, 'I' represents an increasing relationship between two numbers. And our **secret signature **was constructed by a special integer array, which contains uniquely all the different number from 1 to n \(n is the length of the secret signature plus 1\). For example, the secret signature "DI" can be constructed by array \[2,1,3\] or \[3,1,2\], but won't be constructed by array \[3,2,4\] or \[2,1,3,4\], which are both illegal constructing special string that can't represent the "DI" **secret signature**.
>
> On the other hand, now your job is to find the lexicographically smallest permutation of \[1, 2, ... n\] could refer to the given **secret signature **in the input.
>
> **Example 1:**
>
> ```
> Input:
> "I"
>
> Output:
> [1,2]
>
> Explanation:
> [1,2] is the only legal initial spectial string can construct secret signature "I", where the number 1 and 2 construct an increasing relationship.
> ```
>
> **Example 2:**
>
> ```
> Input:
> "DI"
>
> Output:
> [2,1,3]
>
> Explanation:
> Both [2,1,3] and [3,1,2] can construct the secret signature "DI",
> but since we want to find the one with the smallest lexicographical permutation, you need to output [2,1,3]
> ```
>
> **Note:**
>
> * The input string will only contain the character 'D' and 'I'.
> * The length of input string is a positive integer and will not exceed 10,000

### 思路

观察题目，因为需要输出的结果是lexicographically，所以需要结果中从左到右的num尽可能的小。用num表示该插入结果的值。

那么只有2种情况：

1. 遇见“I”的时候，直接把此时的num放入结果中对应位置即可。
2. 遇见"D"，甚至好多个**连续**“D”的时候，比如“...IDDDI...”，那么为了保证第一个D是当前所能用的num中最小的，所以可以从最后1个D开始放入num，然后num++，放入倒数第二个D的位置，然后num++，放入第一个D的位置。

### 复杂度

O\(N\), N是input string的长度

### Code

```java
public class Solution {
    public int[] findPermutation(String s) {
        int n = s.length() + 1;
        int[] rst = new int[n];
        int num = 1;
        int i = 0;
        while(i < s.length()) {
            if(s.charAt(i) == 'I') rst[i++] = num++;
            else {
                int j = i;
                while(j < s.length() && s.charAt(j) == 'D') j ++;
                for(int k = j; k >= i; k--) rst[k] = num++; 
                // Trick注意: 这里需要 i 值set为 (j+1)。因为这里j只有2种情况:
                // 1. j == s.length()，那么说明rst中元素已经填满，此题结束
                // 2. 说明 s.charAt(j) == 'I'，由于num是从小到大放入rst的，所以下1个num一定比rst[j]大。下一步直接check s.charAt(j+1)的值来决定rst[j+1]的值
                i = j + 1;
            }
        }
        // 比如最后一个字母为“I”，这里rst的length比s的length大一，rst并没有放满number
        // 所以这里可能需要放入最后一个元素
        if(i < n) rst[i] = num;
        return rst;
    }
}
```



