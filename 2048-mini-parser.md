# 385. Mini Parser

> Given a nested list of integers represented as a string, implement a parser to deserialize it.
>
> Each element is either an integer, or a list -- whose elements may also be integers or other lists.
>
> **Note: **You may assume that the string is well-formed:
>
> * String is non-empty.
> * String does not contain white spaces.
> * String contains only digits "0-9", "\[", "-", "," , "\]".
>
> **Example 1:**
>
> ```
> Given s = "324",
>
> You should return a NestedInteger object which contains a single integer 324.
> ```
>
> **Example 2:**
>
> ```
> Given s = "[123,[456,[789]]]",
>
> Return a NestedInteger object containing a nested list with 2 elements:
>
> 1. An integer containing value 123.
> 2. A nested list containing two elements:
>     i.  An integer containing value 456.
>     ii. A nested list with one element:
>          a. An integer containing value 789.
> ```

### 思路

这里是一个嵌套的过程，就是遇见 “ \[ ” 的时候，需要嵌套新的nested integer 到前面nested integer当中。所以需要一个数据结果来存之前 nested integer，这种“嵌套”的感觉容易想到stack。

### Code

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public NestedInteger deserialize(String s) {
        if(s.length() == 0) return new NestedInteger();
        
        // input string没有括号，只有数字的情况
        if(s.charAt(0) != '[') return new NestedInteger(Integer.valueOf(s));

        Stack<NestedInteger> stack = new Stack();
        NestedInteger root = new NestedInteger();
        stack.push(root);
        
        // 不用遍历到最后一位(s.length() - 1)，因为那位是"]"，留着最后stack.pop 出来root是最后结果
        for(int i = 1; i < s.length() - 1; i ++) {
            char c = s.charAt(i);
            if(c == '[') {
                NestedInteger cur = new NestedInteger();
                // 把current nested 嵌套入上一个nested当中
                stack.peek().add(cur);
                // 把current nested push进去，因为后面还可能有nested需要嵌入到当前nested中
                stack.push(cur);
            } else if (c == ',') {
                continue;
            } else if (c == ']') {
                stack.pop(); // 右括号时候，表明当前nested已经构建完了，可以pop出
            } else {
                // 读取substring中的数字部分
                int start = i;
                while(i < s.length() - 1 && Character.isDigit(s.charAt(i+1))) i ++;
                int end = i;
                stack.peek().add(new NestedInteger(Integer.valueOf(s.substring(start, end + 1))));
            }
        }
        return stack.pop();
    }
}
```



