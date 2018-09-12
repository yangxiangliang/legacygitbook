# 241. Different Ways to Add Parentheses

> Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are`+`,`-`and`*`.
>
> **Example 1:**
>
> ```
> Input: "2-1-1"
> Output: [0, 2]
> Explanation: 
> ((2-1)-1) = 0 
> (2-(1-1)) = 2
> ```
>
> **Example 2:**
>
> ```
> Input: "2*3-4*5"
> Output: [-34, -14, -10, -10, 10]
> Explanation: 
> (2*(3-(4*5))) = -34 
> ((2*3)-(4*5)) = -14 
> ((2*(3-4))*5) = -10 
> (2*((3-4)*5)) = -10 
> (((2*3)-4)*5) = 10
> ```

### 思路

1. 一开始拿到题目容易没什么方向，感觉加括号的可能性太多，无从下手。往往这种时候，可以考虑divide and conqure 的方法，用recursion。比如在input string中遇见一个符号的时候，用recursion求其符号左边的substring的所有可能的计算结果，然后同样可以求得该符号右边的substring的所有可能的计算结果。然后将这些结果的组合计算后，得到该input string的所有加括号情况的结果了。
2. 为了efficient，用一个hashMap 存已经计算得到的结果，避免重复计算即可。

### Code

```java
class Solution {
    Map<String, List<Integer>> map = new HashMap<>();

    public List<Integer> diffWaysToCompute(String input) {
        if(map.containsKey(input)) return map.get(input);
        List<Integer> rst = new ArrayList<>();

        for(int i = 0; i < input.length(); i ++) {
            char c = input.charAt(i);
            if(c == '+' || c == '-' || c == '*') {
                List<Integer> leftNums = diffWaysToCompute(input.substring(0, i));
                List<Integer> rightNums = diffWaysToCompute(input.substring(i+1, input.length()));

                for(int left : leftNums) {
                    for(int right : rightNums) {
                        if(c == '+') rst.add(left + right);
                        if(c == '-') rst.add(left - right);
                        if(c == '*') rst.add(left * right);
                    }
                }
            }
        }
        
        // corner case, 表明input string中没有符号，只有一个单独的数
        if(rst.size() == 0) rst.add(Integer.valueOf(input));
        map.put(input, rst);
        return rst;
    }
}
```



