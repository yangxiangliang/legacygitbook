# 166. Fraction to Recurring Decimal

> Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.
>
> If the fractional part is repeating, enclose the repeating part in parentheses.
>
> For example,
>
> * Given numerator = 1, denominator = 2, return "0.5".
> * Given numerator = 2, denominator = 1, return "2".
> * Given numerator = 2, denominator = 3, return "0.\(6\)".

### 思路

题目的思路比较简单，就是按照Divide的规则来计算结果即可。则先算 numerator / denominator 的值，然后算remainder = numerator % denominator 的值，如果remainder == 0，则结束。如果remainder 不为 0，则 remainder = remainder % 10，再来计算remainder / denominator 的值，如此下去。关键是看什么时候 fractional part 开始 repeating，当遇见相同的remainder的时候，则fractional part开始repeating了，所以用1个HashMap记录remainder 和 该 remainder对应的计算结果在rst string中index的位置，这样如果之后遇见同一个remainder，那么需要在该index出插入 “\(”。

### 注意

因为numerator, denominator 为int，但是在之后的计算中有 remainder \* 10 直到大于denominator的运算，可能结果超出int，所以一开始将其改为 long 来表示。

### Code

```java
public class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if(numerator == 0) return "0";

        StringBuilder rst = new StringBuilder();
        // 插入 "-" 符号 如果最后结果是负数
        if((long) numerator * denominator  < 0) rst.append('-');
        // 将其转化为 long，因为后面计算可能超出int 范围
        long num = Math.abs((long) numerator);
        long den = Math.abs((long) denominator);
        rst.append(num / den);
        num = num % den;  // 求remainder
        if(num == 0) return rst.toString();    

        rst.append('.');
        HashMap<Long, Integer> map = new HashMap();
        while(num > 0) {
            num = num * 10;
            if(map.containsKey(num)) {
                rst.insert(map.get(num), "(");
                rst.append(")");
                break;
            }
            // 记录该 remainder(num * 10)以及其运算结果对应的在result string中的index的位置
            map.put(num, rst.length());
            rst.append(num / den);
            num = num % den;
        }
        return rst.toString();
    }
}
```



