# 32. Longest Valid Parentheses

> Given a string containing just the characters`'('`and`')'`,  find the length of the longest valid \(well-formed\) parentheses substring.
>
> **Example 1:**
>
> ```
> Input: "(()"
> Output: 2
> Explanation: The longest valid parentheses substring is "()"
> ```
>
> **Example 2:**
>
> ```
> Input: ")()())"
> Output: 4
> Explanation: The longest valid parentheses substring is "()()"
> ```

### 思路

1. 这里要找最长的valid的括号substring，substring肯定是连续的，找括号的问题最容易想到利用stack的存储结构。如果当前是右括号，遇见stack顶部是左括号，则可以pop出该括号。表示找到了与其匹配的括号组合。
2. 那么对于该题目中的valid的括号 substring，需要求其长度，所以stack中不能存character '\(', '\)'，因为这样没有位置信息。所以改存对应character的index值。每pop出一个左括号后，只需要用当前index数值 - 此时stack顶部的index数字，便是当前valid的括号substring的长度，取其最大值即可

### Code

```java
class Solution {
    public int longestValidParentheses(String s) {
        int rst = 0;
        Stack<Integer> stack = new Stack();
        for(int i = 0; i < s.length(); i ++) {
            if(s.charAt(i) == '(') stack.push(i);
            else {
                if(stack.isEmpty() || s.charAt(stack.peek()) == ')') stack.push(i);
                else {
                    stack.pop();
                    rst = Math.max(rst, i - (stack.isEmpty() ? -1 : stack.peek()));
                }
            }
        }
        return rst;
    }
}
```



