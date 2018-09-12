# 266. Palindrome Permutation

> Given a string, determine if a permutation of the string could form a palindrome.
>
> For example,  
> `"code"`-&gt; False,`"aab"`-&gt; True,`"carerac"`-&gt; True.

### 思路

用set遍历string中的所有character，如果set中已经有，则remove掉；反之，则加入set中，如果有可能组成valid palindrome，那么最后set中只有可能剩1个char，或则一个char都没有。

### Code

```java
public class Solution {
    public boolean canPermutePalindrome(String s) {
        HashSet<Character> set = new HashSet();
        for(char c : s.toCharArray()) {
            if(set.contains(c)) set.remove(c);
            else set.add(c);
        }
        return set.size() <= 1;
    }
}
```



