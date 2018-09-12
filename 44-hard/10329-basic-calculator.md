# 224. Basic Calculator

> Implement a basic calculator to evaluate a simple expression string.
>
> The expression string may contain open`(`and closing parentheses`)`, the plus`+`or minus sign`-`, **non-negative **integers and empty spaces.
>
> You may assume that the given expression is always valid.
>
> Some examples:
>
> ```
> "1 + 1" = 2
> " 2-1 + 2 " = 3
> "(1+(4+5+2)-3)+(6+8)" = 23
> ```
>
> **Note: Do not **use the `eval`built-in library function.

### 思路

因为这个运算中有括号，感觉有括号的地方下意识就会想到需不需要用Stack来解决，这里确实可以用Stack。基本思路就是遇见digit的时候，就把其表示的number push 入stack，但是要注意有些number可能有好几个digit，而不是只有1个digit。还有要注意的就是遇见"-" 号，需要push 入 stack，因为可以默认在stack里的number都是相加，但是遇见"-"符号的时候，需要减去这个number。

### Code 1\)

```java
public class Solution {
    public int calculate(String s) {
        Stack<String> stack = new Stack();

        for(int i = 0; i < s.length(); i ++) {
            char c = s.charAt(i);
            if ( c == '-') stack.push("-");
            else if (Character.isDigit(c)) {
                // 计算当前的number值，可能有好几个digit
                int count = Character.getNumericValue(c);
                while(i < s.length() - 1 && Character.isDigit(s.charAt(i+1))) {
                    count = 10 * count + Character.getNumericValue(s.charAt(i+1));
                    i ++;
                }
                stack.push(""+count);
            }
            else if ( c == '(') stack.push("(");
            else if( c == ')') {
                // 计算这一段"(....)" 括号中的值，然后把结果push到stack当中
                int count = calculateStack(stack);
                stack.pop(); // pop out "("
                stack.push(""+count);
            }
        }
        return calculateStack(stack);
    }

    // calculate current stack element until stack is empty or meet "("
    public int calculateStack(Stack<String> stack) {
        int count = 0;

        while(!stack.isEmpty() && !stack.peek().equals("(")) {
            String tmp = stack.pop();
            if(!stack.isEmpty() && stack.peek().equals("-")) {
                // 表示需要从结果中减去当前这个number，其他情况为加上该number
                stack.pop(); // pop out "-"
                count -= Integer.parseInt(tmp);
            } else count += Integer.parseInt(tmp);
        }
        return count;
    }
}
```

### Code 2\)

* 基本思想和上面类似，但是在实现时更简洁。这里的stack直接存Integer，而不是string。\*\*用1个 int sign 来处理符号的问题，sign = 1 表示一般的情况，即符号为"+"，sign = -1 表示当前符号为“-”的情况。
* 遇见"\("的时候，表示新的一段"\(....\)" 要开始了，把当前的计算结果push入stack中，同时把新的这一段的符号情况也push 入 stack中。并且记住需要reset 记录结果的result和sign的值。到遇见"\)"的时候，因为此时stack.pop\(\)第一个值表示这一段"\(...\)"的符号，所以当前结果 \* stack.pop\(\)的符号，然后再加上stack.pop\(\)第二个的值，因为第二个值表示之前的运算结果。

* ```java
  public class Solution {
    public int calculate(String s) {
        int rst = 0, sign = 1;
        Stack<Integer> stack = new Stack();

        for(int i = 0; i < s.length(); i ++) {
            char c = s.charAt(i);
            if ( c == '-') sign = -1;
            else if (c == '+') sign = 1;
            else if (Character.isDigit(c)) {
                int count = Character.getNumericValue(c);
                while(i < s.length() - 1 && Character.isDigit(s.charAt(i+1))) {
                    count = 10 * count + s.charAt(i+1) - '0';
                    i ++;
                }
                rst += count * sign;
            }
            else if ( c == '(') {
                stack.push(rst);
                stack.push(sign);
                rst = 0;
                sign = 1;
            }
            else if( c == ')') {
                rst = rst * stack.pop() + stack.pop();
            }
        }
        return rst;
    }
  }
  ```



