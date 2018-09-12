# 763. Partition Labels

> A string`S`of lowercase letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.
>
> **Example 1:**
>
> ```
> Input: S = "ababcbacadefegdehijhklij"
> Output: [9,7,8]
> Explanation:
> The partition is "ababcbaca", "defegde", "hijhklij".
> This is a partition so that each letter appears in at most one part.
> A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
> ```

### 思路

1. 因为题目中说了只有小写字母，又因为每个字母最多只能在一个part里，而且要尽可能多分割parts。所以想到要记录每个字母在string中end处的index，因为只有26个小写字母，所以用个长度26的array即可。
2. 然后需要scan该string，这里需要记录扫过的character的end处并且取最大值，end处等于当前 index 的时候，则划分为一个part即可。

### Code

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        int[] ends = new int[26]; // 记录每个character的end，即最大的index值
        for(int i = 0; i < S.length(); i ++) {
            ends[S.charAt(i) - 'a'] = i;
        }

        int end = 0;
        int start = 0;
        List<Integer> rst = new ArrayList();
        for(int i = 0; i < S.length(); i ++) {
            end = Math.max(end, ends[S.charAt(i) - 'a']);
            if(end == i) {
                rst.add(end - start + 1);
                start = end + 1;
            }
        }
        return rst;
    }
}
```



