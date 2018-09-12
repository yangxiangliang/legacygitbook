# 171. Excel Sheet Column Number

> Related to question [Excel Sheet Column Title](https://leetcode.com/problems/excel-sheet-column-title/)
>
> Given a column title as appear in an Excel sheet, return its corresponding column number.
>
> **For example:**
>
> ```
>     A -> 1
>     B -> 2
>     C -> 3
>     ...
>     Z ->  26
>     AA -> 27
>     AB -> 28
> ```

### 思路

一共有26个英文字母，AA = 26 + 1， AB = 26 + 2...... 其实就相等于是一个“26 进制” 的东西，“A”是起始点，作为1；“Z”相当于26

### Code

```java
class Solution {
    public int titleToNumber(String s) {
        int rst = 0;
        int start = 0;
        while(start < s.length()) {
            int tmp = s.charAt(start) - 'A' + 1;
            rst = 26 * rst + tmp;
            start++;
        }
        return rst;
    }
}
```



