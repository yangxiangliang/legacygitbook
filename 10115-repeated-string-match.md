# 686. Repeated String Match

> Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.
>
> For example, with A = "abcd" and B = "cdabcdab".
>
> Return 3, because by repeating A three times \(“abcdabcdabcd”\), B is a substring of it; and B is not a substring of A repeated two times \("abcdabcd"\).

### 思路

按照题目的意思分析，不断repeat的append string A，直到其长度大于B的时候，看当前的string是否contain string B。如果不行，则表示无法repeat string A来得到contain string B的字符串。

### Code

```java
class Solution {
    public int repeatedStringMatch(String A, String B) {
        StringBuilder rst = new StringBuilder();
        int count = 0;
        while(rst.length() < B.length()) {
            rst.append(A);
            count ++;
            if(rst.toString().contains(B)) return count;
        }
        
        count ++;
        rst.append(A);
        if(rst.toString().contains(B)) return count;
        return -1;
    }
}
```



