# 20. Valid Parentheses

> Given a string containing just the characters`'('`,`')'`,`'{'`,`'}'`,`'['`and`']'`, determine if the input string is valid.
>
> The brackets must close in the correct order,`"()"`and`"()[]{}"`are all valid but`"(]"`and`"([)]"`are not.

### 思路

用1个stack，当都是左括号的时候，push入stack，当是右括号的时候，peek stack的元素，如果是左括号，则pop stack；反之，则把该右括号push入stack。如果input的是valid，那么最后的stack应该是空。

### Code

```java
public class Solution {
    HashMap<Character, Character> map = new HashMap();
    
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack();
        for(String tmp : new String[]{"()", "{}", "[]"}) map.put(tmp.charAt(1), tmp.charAt(0));
        
        for(char c : s.toCharArray()) {
            if(stack.isEmpty() || c == '(' || c == '{' || c == '[') stack.push(c);
            else if(stack.peek() == map.get(c)) stack.pop();
            else stack.push(c);
        }
        return stack.isEmpty();
    }
}
```



