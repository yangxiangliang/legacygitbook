# 415. Add Strings

> Given two non-negative integers `num1`and `num2`represented as string, return the sum of `num1`and `num2`.
>
> **Note:**
>
> 1. The length of both _num1_ and _num2_ is &lt; 5100.
>
> 2. Both _num1_ and _num2_ contains only digits _0-9_.
>
> 3. Both _num1_ and _num2_ does not contain any leading zero.
>
> 4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

### 思路

就是将2个string中的char一位一位相加，然后用一个int carry记录进位的情况，将相加的结果append到构建结果的string builder中，因为most significant 在string builder的最右边，所以最后reverse一下结果的string builder即可。

### Code

```java
public class Solution {
    public String addStrings(String num1, String num2) {
        StringBuilder rst = new StringBuilder();
        int index1 = num1.length() - 1;
        int index2 = num2.length() - 1;
        int carry = 0;
        while(index1 >= 0 || index2 >= 0) {
            int tmp;
            if(index1 >= 0 && index2 >= 0) tmp = (num1.charAt(index1--) - '0') + (num2.charAt(index2--) - '0') + carry;
            else if(index1 >= 0) tmp = (num1.charAt(index1--) - '0') + carry;
            else tmp = (num2.charAt(index2--) - '0') + carry;
            rst.append(tmp % 10);
            carry = tmp / 10;
        }
        if(carry > 0) rst.append(carry);
        return rst.reverse().toString();
    }
}
```



