# 89. Gray Code

> The gray code is a binary numeral system where two successive values differ in only one bit.
>
> Given a non-negative integer \_n \_representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.
>
> For example, given _n _= 2, return`[0,1,3,2]`. Its gray code sequence is:
>
> ```
> 00 - 0
> 01 - 1
> 11 - 3
> 10 - 2
> ```
>
> **Note:**
>
> For a given _n_, a gray code sequence is not uniquely defined.
>
> For example,`[0,2,3,1]`is also a valid gray code sequence according to the above definition.
>
> For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

### 思路

1. 注意观察gray code的规律, 比如 n = 1的gray code 就是\[0, 1\]; n = 2 的 gray code \[00, 01, 11, 10\] 其实就是在\[0, 1\] 前面先都加上0，变成\[00, 01\]。然后把\[0, 1\] reverse一下变成\[1, 0\]，在其前面都加上1，变成\[11, 10\]。同理 n = 3的gray code 也可以根据 n = 2的gray code 如此变化而得到。
2. 所以如果需要求 n 的gray code，那么先求得 \(n-1\)的gray code。然后把 \(n-1\)的gray code 前面加上0\(其实相当于不加\)，就是n的gray code的前半部分。然后把\(n-1\)的gray code 顺序reverse 一下，在每个数的最前面加1，便得到 n 的gray code的后半部分。

### Code

```java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> rst = new ArrayList();
        if(n <= 1) {
            for(int i = 0; i <= n; i ++) rst.add(i);
            return rst;
        }
        
        List<Integer> pre = grayCode(n - 1);
        rst.addAll(pre);
        
        // incre 相当于就是把(n-1)的gray code reverse后在前面加上的那个"1"
        int incre = 1 << (n-1);
        for(int i = pre.size() - 1; i >= 0; i --) {
            rst.add(incre + pre.get(i));
        }
        return rst;
    }
}
```



