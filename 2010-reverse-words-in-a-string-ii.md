# 186. Reverse Words in a String II

> Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.
>
> The input string does not contain leading or trailing spaces and the words are always separated by a single space.
>
> For example,  
> Given s = "`the sky is blue`",  
> return "`blue is sky the`".
>
> Could you do itin-placewithout allocating extra space?
>
> Related problem:[Rotate Array](https://leetcode.com/problems/rotate-array/)
>
> **Update \(2017-10-16\):**  
> We have updated the function signature to accept a`character array`, so please **reset to the default code definition **by clicking on the reload button above the code editor. Also, **Run Code **is now available!

### 思路

1. 这里一开始最容易想到的就是把sentence里面的char从头到尾swap一下。但是swap后发现结果是“eulb si yks eht”的样子，即每个Word也是reverse的，所以还需要对每个Word reverse一次得到最终结果。

### Code

```java
class Solution {
    public void reverseWords(char[] str) {
        reverse(str, 0, str.length-1);
        int start = 0, end = 0;
        // 避免str[end+1] 超出bound，所以这里limit是(str.length - 1)
        for(end = 0; end < str.length-1; end ++) {
            if(str[end+1] == ' ') {
                reverse(str, start, end);
                start = end + 2;
            }
        }
        if(end == str.length-1) reverse(str, start, end);
    }

    private void reverse(char[] str, int start, int end) {
        while(start < end) {
            char tmp = str[start];
            str[start] = str[end];
            str[end] = tmp;
            start++;
            end--;
        }
    }
}
```



