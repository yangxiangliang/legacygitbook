# 22. Generate Parentheses

> Given _n_  pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
>
> For example, given _n_ = 3, a solution set is:
>
> ```
> [
>     "((()))",
>     "(()())",
>     "(())()",
>     "()(())",
>     "()()()"
> ]
> ```

### 思路

典型的backtracking，用一个builder helper function，每一次parse还剩下多少个left 和 right 括号，如果 个数小于0或则left 大于 right，说明无法得到合适的结果，则不用继续。当所有括号用完的时候，则把构造的string加入结果。

### 复杂度

**时间：  **也是backtracking，分析下最后的解空间树，因为return的string的每个char可能有2种选择，即'\(' or '\)'。解空间复杂度应该是O\(2^N\)，时间复杂度也是O\(2^N\)

### Code

```java
public class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> rst = new ArrayList();
        build(rst, n, n, "");
        return rst;
    }

    public void build(List<String> list, int left, int right, String pre) {
        if(left > right || left < 0 || right < 0) return;
        if(left == 0 && right == 0) {
            list.add(pre);
            return;
        }

        build(list, left - 1, right, pre + "(");
        build(list, left, right - 1, pre + ")");
    }
}
```



