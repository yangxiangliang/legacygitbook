# 17. Letter Combinations of a Phone Number

> Given a digit string, return all possible letter combinations that the number could represent.
>
> A mapping of digit to letters \(just like on the telephone buttons\) is given below.
>
> ![](/assets/import.png)
>
> ```
> Input: Digit string "23"
>
> Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
> ```
>
> **Note:**
>
> Although the above answer is in lexicographical order, your answer could be in any order you want.

### 思路

基本的Backtracking，用一个helper function，遍历 digits string从左到右的所有digit character，对于每个digit character，加上其对应的character，直到digits string结束。

### 复杂度

时间：典型的backtracking感觉，因为空间解的可能性是\(3^n\)，3是每个digit对应的char的平均个数，n是digits string的长度，每个digit对应3个可能性，所以最后空间解的个数是\(3^n\)。时间复杂度也应该是O\(3^n\)。

### Code

```java
public class Solution {
    // map[index] 对应的就是 num为index的电话号码，对应的所有character
    String[] map = new String[]{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    public List<String> letterCombinations(String digits) {
        List<String> rst = new ArrayList();
        if(digits.length() == 0) return rst;
        helper(rst, 0, digits, "");
        return rst;
    }

    public void helper(List<String> rst, int index, String digits, String pre) {
        if(index == digits.length()) {
            rst.add(pre);
            return;
        }

        int num = Character.getNumericValue(digits.charAt(index));
        for(char c : map[num].toCharArray()) 
            helper(rst, index+1, digits, pre+c);
    }
}
```



