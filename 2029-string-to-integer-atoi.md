# 8. String to Integer \(atoi\)

> Implement`atoi`to convert a string to an integer.
>
> **Hint: **Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.
>
> **Notes: **It is intended for this problem to be specified vaguely \(ie, no given input specs\). You are responsible to gather all the input requirements up front.
>
> **Requirements for atoi:**
>
> The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.
>
> The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.
>
> If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.
>
> If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT\_MAX \(2147483647\) or INT\_MIN \(-2147483648\) is returned.

### 思路

1. 思路很容易，就是用pointer指向string的每一个char，从左到右。首先skip掉space和符号位。然后往下走的时候，如果不是digit位置，就返回当前结果值即可。
2. **注意 ** 还要处理超出int范围的情况，这里采用先用long来存结果，没走一步就long和int的边界值比较，如果超出了，就返回int的边界值即可。

### Code

```java
class Solution {
    public int myAtoi(String str) {
        long rst = 0;
        int sign = 1;
        int start = 0, len = str.length();

        // skip white space
        while(start < len && str.charAt(start) == ' ') start ++;
        if (start == len) return 0;

        // check sign
        if(str.charAt(start) == '-' || str.charAt(start) == '+') {
            if(str.charAt(start) == '-') sign = -1;
            start ++;
        }

        while(start < len) {
            if(!Character.isDigit(str.charAt(start))) break;
            long tmp = Character.getNumericValue(str.charAt(start));
            rst = 10 * rst + tmp;
            // 每一步都要检查 long rst和int的边界值，不能最后在while之外来检查，因为rst可能过程中已经越过long的边界
            // 造成最后rst的值也不准确。而且当中检查，可以避免越界了还做无用的计算
            if(sign == -1 && (-1) * rst < Integer.MIN_VALUE) return Integer.MIN_VALUE;
            if(sign == 1 && rst > Integer.MAX_VALUE) return Integer.MAX_VALUE;
            start ++;
        }
        return (int) (sign * rst);
    }
}
```



