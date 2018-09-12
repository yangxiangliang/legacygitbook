# 400. Nth Digit

> Find the _Nth_ digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...
>
> **Note:**
>
> _**n**_ is positive and will fit within the range of a 32-bit signed integer \( n &lt; 2^31\).
>
> **Example 1:**
>
> ```
> Input:
> 3
>
> Output:
> 3
> ```
>
> **Example 2:**
>
> ```
> Input:
> 11
>
> Output:
> 0
>
> Explanation:
> The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
> ```

### 思路

这里观察可知，将infinite的sequence分段，分为1-9，10-99，100-999。。。相同位数digit的数字放在1个group里，那么就是要找到Nth digit在哪个group，然后计算其在这个group里的位置，加上group的其实元素，可以得知N th digit的值。

### 注意

最后如果确定Nth Digit 在 XX..XX中时，巧妙的方法是将XX...XX转化为String，然后找其中ith digit 最方便。

### Code

```java
public class Solution {
    public int findNthDigit(int n) {
        long count = 9; // count可能会超出 int范围，这里用long
        int len = 1;
        int start = 1;
        while(n > count * len) {
            n -= len * count;
            len ++;
            count = count * 10;
            start = start * 10;
        }
        
        start += (n - 1) / len;
        // Trick注意: convert start to string to get Nth digit easily
        String tmp = Integer.toString(start);
        return Character.getNumericValue(tmp.charAt((n -1 ) % len));
    }
}
```



