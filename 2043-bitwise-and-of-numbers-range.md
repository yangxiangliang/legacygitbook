# 201. Bitwise AND of Numbers Range

> Given a range \[m, n\] where 0 &lt;= m &lt;= n &lt;= 2147483647, return the bitwise AND of all numbers in this range, inclusive.
>
> **Example 1:**
>
> ```
> Input: [5,7]
> Output: 4
> ```
>
> **Example 2:**
>
> ```
> Input: [0,1]
> Output: 0
> ```

### 思路

1. brute force的方法，m 和 \(m+1\) 每一位and运算，然后和 \(m+2\) and运算，直到和n做and运算，得到结果。Time Complexity是O\(N\)。太慢。

2. 那么找有没有更快的方法，所以需要找规律，看 \[m, n\] range 中所有数相互AND后是什么结果。只要m和n不相等，那么一定会出现 n = xyz1...,  m = xyz0... 的情况。即n和m的前面几位都是相同的，然后从某一位开始，较大的n的该位置为1，较小的m的该位置为0。注意观察n, m 范围里面一定会出现2个这样的情况：xyz100...0, xyz0111...1. 即所有m, n之间的数bitwise AND后，肯定是xyz000...0, 即我们只需要找到n, m 从左边bit到右边都相等的bit的长度。即每次把n, m 右移动一位，直到两个值相等。然后记录移动的步数x，再把结果左移动x bit 便是结果。

### Code

```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        int moved = 0;
        while(m != n) {
            m >>= 1;
            n >>= 1;
            moved ++;
        }
        return m << moved;
    }
}
```



