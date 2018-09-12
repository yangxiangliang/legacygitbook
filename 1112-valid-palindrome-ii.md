# 680. Valid Palindrome II

> Given a non-empty string`s`, you may delete **at most **one character. Judge whether you can make it a palindrome.  
> **Example 1:**
>
> ```
> Input: "aba"
> Output: True
> ```
>
> **Example 2:**
>
> ```
> Input: "abca"
> Output: True
> Explanation: You could delete the character 'c'.
> ```
>
> **Note:**
>
> 1. The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

### 思路

检查palindrome的常用方法就是2个首尾pointer分别从string的头尾开始往中间移动，看在每个位置是否相等即可。这里因为可以最多删除一个char，所以在移动start, end 2个pointer时候，当遇见对应的某2个位置char不相同的时候，不着急返回false。那么这里有2种情况讨论，1个是可能删除start pointer处的char，另一种情况是删除end pointer处的char。以上2种情况中，只要某一种情况中，剩下的substring是palindrome的话，那么return true即可。

### Code

```java
class Solution {
    public boolean validPalindrome(String s) {
        int start = 0, end = s.length() - 1;
        while(start < end) {
            if(s.charAt(start) != s.charAt(end)) {
                if(isValid(s.substring(start+1, end+1)) || isValid(s.substring(start, end))) return true;
                return false;
            }
            start ++;
            end --;
        }
        return true;
    }
    
    private boolean isValid(String s) {
        int start = 0, end = s.length() - 1;
        while(start < end) {
            if(s.charAt(start++) != s.charAt(end--)) return false;
        }
        return true;
    }
}
```



