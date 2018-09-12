# 422. Valid Word Square

> Given a sequence of words, check whether it forms a valid word square.
>
> A sequence of words forms a valid word square if the _kth_ row and column read the exact same string, where 0 ≤k&lt; max\(numRows, numColumns\).
>
> **Note:**
>
> 1. The number of words given is at least 1 and does not exceed 500.
> 2. Word length will be at least 1 and does not exceed 500.
> 3. Each word contains only lowercase English alphabet a-z.
>
> **Example 1:**
>
> ```
> Input:
> [
>   "abcd",
>   "bnrt",
>   "crmy",
>   "dtye"
> ]
>
> Output:
> true
>
>
> Explanation:
> The first row and first column both read "abcd".
> The second row and second column both read "bnrt".
> The third row and third column both read "crmy".
> The fourth row and fourth column both read "dtye".
>
> Therefore, it is a valid word square.
> ```
>
> **Example 2:**
>
> ```
> Input:
> [
>   "abcd",
>   "bnrt",
>   "crm",
>   "dt"
> ]
>
> Output:
> true
>
>
> Explanation:
> The first row and first column both read "abcd".
> The second row and second column both read "bnrt".
> The third row and third column both read "crm".
> The fourth row and fourth column both read "dt".
>
> Therefore, it is a valid word square.
> ```
>
> **Example 3:**
>
> ```
> Input:
> [
>   "ball",
>   "area",
>   "read",
>   "lady"
> ]
>
>
> Output:
> false
>
>
> Explanation:
> The third row reads "read" while the third column reads "lead".
>
> Therefore, it is NOT a valid word square.
> ```

### 思路

思路很简单，如果是符合要求的square，那么第 i 个word的第 j 个character必须和第 j 个Word的第 i 个character相同。因为这里的word的长度不相等，所以注意检查i, j 是否超出边界。

### Code

```java
public class Solution {
    public boolean validWordSquare(List<String> words) {
        for(int i = 0;  i < words.size(); i ++) {
            String cur = words.get(i);
            for(int j = 0; j < cur.length(); j ++) {
                // j 可能大于等于 words.size(),那么没有 words.get(j)
                if(words.size() <= j) return false;
                String tmp = words.get(j);
                // 先检查 i 是否在 words.get(j)的string的长度范围内
                if(tmp.length() < i + 1 || tmp.charAt(i) != cur.charAt(j)) return false;
            }
        }
        return true;
    }
}
```



