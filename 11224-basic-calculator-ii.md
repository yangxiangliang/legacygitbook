# 227. Basic Calculator II

> Implement a basic calculator to evaluate a simple expression string.
>
> The expression string contains only **non-negative **integers,`+`,`-`,`*`,`/`operators and empty spaces. The integer division should truncate toward zero.
>
> **Example 1:**
>
> ```
> Input: "3+2*2"
> Output: 7
> ```
>
> **Example 2:**
>
> ```
> Input: " 3/2 "
> Output: 1
> ```
>
> **Example 3:**
>
> ```
> Input: " 3+5 / 2 "
> Output: 5
> ```
>
> **Note:**
>
> * You may assume that the given expression is always valid.

### 思路

1. 用1个stack记录所有的integer，同时记录当前最近的符号sign。注意，如果sign == '-'，压入当前number的时候，需要压入其负数值。如果遇见sign 是 乘法或则除法，因为优先级更高，所以把stack顶端元素pop出来和当前元素先计算，再把结果压入stack中。

### Code

```java
class Solution {
    public int calculate(String s) {
        Stack<Integer> stack = new Stack();
        
        char sign = '+'; // 初始sign的值
        for(int i = 0; i < s.length(); i ++) {
            if(s.charAt(i) == ' ') continue; // skip掉空格
            
            if(Character.isDigit(s.charAt(i))) {
                int tmp = 0;
                while( i < s.length() && Character.isDigit(s.charAt(i))) {
                    tmp = 10 * tmp + (s.charAt(i) - '0');
                    i ++;
                }
                i --; // 因为每个for循环后 i ++，所以这里 i-- 抵消掉上面while循环最后一次的i ++
                if(sign == '+') stack.push(tmp);
                else if (sign == '-') stack.push(-tmp);
                else if (sign == '/') stack.push(stack.pop() / tmp);
                else if (sign == '*') stack.push(stack.pop() * tmp);
            } else sign = s.charAt(i);
        }
        
        int rst = 0;
        while(!stack.isEmpty()) rst += stack.pop();
        return rst;
    }
}
```



