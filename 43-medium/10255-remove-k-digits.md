# 402. Remove K Digits

> Given a non-negative integer _num_ represented as a string, remove _k_ digits from the number so that the new number is the smallest possible.
>
> **Note:**
>
> * The length of _num_ is less than 10002 and will be &gt;= k.
>
> * The given _num_ does not contain any leading zero.
>
> **Example 1:**
>
> ```
> Input: num = "1432219", k = 3
> Output: "1219"
> Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
> ```
>
> **Example 2:**
>
> ```
> Input: num = "10200", k = 1
> Output: "200"
> Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
> ```
>
> **Example 3:**
>
> ```
> Input: num = "10", k = 2
> Output: "0"
> Explanation: Remove all the digits from the number and it is left with nothing which is 0.
> ```

### 思路

首先要想清楚怎么样remove掉 k digits后得到的数是最小的，或则说怎么确定某个位置的number是需要remove掉还是保留下来的。

* 首先一开始从左到右遍历，尽量保持左边的digit尽量小，因为左边digit为more significant digit
* 比如位置i 的number 大于 \(i+1\)，那么显然需要remove掉位置i的digit，得到尽量小的结果

**这里使用一个stack**,  记录当前加入结果的digit，从左到右遍历input string num，如果stack peek的digit，即其最右边的digit大于num中当前的digit，那么需要remove掉stack peek处的digit，直到stack peek处的digit 不大于 num中当前digit。换句话说，也就是让stack中digit尽量保持一个non-descending的序列。遍历一次结束后，如果还有需要remove的digit，那么因为stack中的digit是non-descending的，最右边的digit即是最大的，也就是stack peek除的digit，那么从stack peek处开始remove直到remove掉所有K个digit为止。

### Code

```java
public class Solution {
    public String removeKdigits(String num, int k) {
        int len = num.length();
        // 处理 Corner cases
        if(k == len) return "0";
        if(k == 0) return num;

        Stack<Character> stack = new Stack();
        int i = 0;
        while(i < len) {
            // 如果stack peek的number大于当前num中的character，则pop出stack中的数
            // 换句话说，就是让stack中保持一个non-descending的序列
            while(k > 0 && !stack.isEmpty() && stack.peek() > num.charAt(i)) {
                stack.pop();
                k --;
            }
            stack.push(num.charAt(i++));
        }

        // 如果此时k > 0，说明还有digit需要remove掉，因为此时stack中是non-descending的序列
        // 需要pop出尽量大的character，所以从stack的peek处开始pop元素直到 k == 0
        while(k > 0) {
            stack.pop();
            k --;
        }

        StringBuilder rst = new StringBuilder();
        while(!stack.isEmpty()) rst.append(stack.pop());
        // 由于num中character放入stack的顺序，从stack中pop出的char组成的rst是倒序的，需要reverse一下
        rst.reverse();

        // remove掉rst 开头的leading Zeros
        while(rst.length() > 0 && rst.charAt(0) == '0') rst.deleteCharAt(0);
        return rst.length() == 0 ? "0" : rst.toString();
    }
}
```



