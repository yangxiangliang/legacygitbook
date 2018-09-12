# 125. Valid Palindrome

> Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
>
> For example,
>
> `"A man, a plan, a canal: Panama"`is a palindrome.
>
> `"race a car"`is \_not \_a palindrome.
>
> **Note:**
>
> Have you consider that the string might be empty? This is a good question to ask during an interview.
>
> For the purpose of this problem, we define empty string as valid palindrome.

### 思路

思路很容易，因为就是看前后的character对应是否相同。**注意去掉不是alphanumertic的character，然后ignore lower case or upper case **。 所以用2个pointer分别指向前后即可。

**注意写code的时候，注意判断start，end pointers 越界的情况**

### Code

```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.length() == 0) return true;
        int start = 0, end = s.length() - 1;
        while(start < end) {
            // 注意判断 start, end会否越界的情况。这里skip掉不是alphanumeric的char
            while(start < end && !Character.isLetterOrDigit(s.charAt(start))) start ++;
            while(end > start && !Character.isLetterOrDigit(s.charAt(end))) end --;
            if(start < end && Character.toLowerCase(s.charAt(start)) != Character.toLowerCase(s.charAt(end))) return false;
            start ++;
            end --;
        }
        return true;
    }
}
```



