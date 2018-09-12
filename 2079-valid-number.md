# 65. Valid Number

> Validate if a given string is numeric.
>
> ```
> Some examples:
> "0" => true
> " 0.1 " => true
> "abc" => false
> "1 a" => false
> "2e10" => true
> ```
>
> **Note:** It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one.

### 思路

用几个flag 表示是否已经发现dot \('.'\)，'e' , '+'， ‘-’ 和 普通数字的情况，因为根据题目，其他符号应该视为invalid的。然后还要注意这几个符号的出现次数也应该有要求，比如 'e' 出现后面应该是整数，而不不应该有'.'。\`e\` 出现前面也应该用数字出现，而且不能已经有另外一个‘e’ 出现了的情况。‘+’, '-' 情况只能在最开始，或则 ‘e’ 后面出现。

### Code

```java
class Solution {
    public boolean isNumber(String s) {
        s = s.trim(); // 去掉 头尾的space
        int index = 0;
        boolean num = false; // 最后结果
        boolean exp = false; // 表示是否出现 'e'
        boolean dot = false; // 表示是否出现dot '.'
        
        while(index < s.length()) {
            if(Character.isDigit(s.charAt(index))) {
                num = true;
            } else if (s.charAt(index) == '.') {
                if(dot || exp) return false;
                dot = true;
            } else if (s.charAt(index) == 'e') {
                if(exp || !num) return false;
                exp = true;
                num = false; // num set为false，因为 e 后面必须还要有数字出现，才算valid
            } else if (s.charAt(index) == '+' || s.charAt(index) == '-') {
                if(index != 0 && s.charAt(index - 1) != 'e') return false;
            } else return false;
            index ++;
        }
        return num;
    }
}
```



