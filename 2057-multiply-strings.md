# 43. Multiply Strings

> Given two non-negative integers `num1`and `num2`represented as strings, return the product of `num1`and `num2`, also represented as a string.
>
> **Example 1:**
>
> ```
> Input: num1 = "2", num2 = "3"
> Output: "6"
> ```
>
> **Example 2:**
>
> ```
> Input: num1 = "123", num2 = "456"
> Output: "56088"
> ```
>
> **Note:**
>
> 1. The length of both num1 and num2 is &lt; 10.
>
> 2. Both num1 and num2 contain only digits 0-9.
>
> 3. Both num1 and num2 do not contain any leading zero, except the number o itself.

### 思路

1. 思路就是“演绎”法，按照2个数相乘的方法来计算即可。便于计算，一开始用 int\[\] rst, rst的length 是 num1.length\(\) + num2.length\(\) 来记录中间的计算结果。
2. 注意num1, num2 中每一位数字相乘的时候其结果的位置应该在int\[\] rst 中的什么位置，容易出错。

### Code

```java
class Solution {
    public String multiply(String num1, String num2) {
        int leng1 = num1.length();
        int leng2 = num2.length();
        int[] rst = new int[leng1 + leng2];
        
        for(int i = leng1 - 1; i >= 0; i--) {
            int carry = 0;
            for(int j = leng2 - 1; j >= 0; j--) {
                int a = (int) (num1.charAt(i) - '0');
                int b = (int) (num2.charAt(j) - '0');
                
                // 注意 nums1.char(i), nums2.char(j) 相乘的结果应该在rst中的[i+j+1]处
                // rst[0] 是most significant位
                int tmp = a * b + carry + rst[i + j + 1];
                rst[i + j + 1] = tmp % 10;
                carry = tmp / 10;
            }
            rst[i] = carry;
        }
        
        int i = 0;
        // skip leading 0
        while(i < rst.length && rst[i] == 0) i ++;
        if(i == rst.length) return "0";
        
        StringBuilder result = new StringBuilder();
        for(int k = i; k < rst.length; k ++) result.append(rst[k]);
        return result.toString();
    }
}
```



