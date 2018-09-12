# 294. Flip Game II

> You are playing the following Flip Game with your friend: Given a string that contains only these two characters:`+`and`-`, you and your friend take turns to flip two **consecutive**`"++"`into`"--"`. The game ends when a person can no longer make a move and therefore the other person will be the winner.
>
> Write a function to determine if the starting player can guarantee a win.
>
> For example, given`s = "++++"`, return true. The starting player can guarantee a win by flipping the middle`"++"`to become`"+--+"`.
>
> **Follow up:**  
> Derive your algorithm's runtime complexity.

### 思路

使用backtracking，用recursion，遍历所有s中"++"换成"--"的情况。假设replace后的string为StringReplaced, 如果StringReplaced不能win，则start的player便可以win。 所以只要有1个StringReplaced 是不能win的，则start player就可以win。

### 复杂度

复杂度分析，假设s的长度为n，最多有\(n-1\)中替换"++"为"--"的情况，替换后的s中"+"的个数最多为\(n-2\)，则: T\(n\) = \(n-1\) \* T\(n-2\) = \(n-1\) \* \(\(n-2\)-1\) \* T\(n-4\) ..... = \(n-1\) \* \(n-3\) \* \(n-5\) \*.... = n !! 

O\(N !!\)

### Code

```java
public class Solution {
    public boolean canWin(String s) {
        if(s.length() < 2) return false;
        for(int i = 0; i < s.length() - 1; i ++) {
            if(s.charAt(i) == '+' && s.charAt(i+1) == '+') {
                String replaced = s.substring(0, i) + "--" + s.substring(i+2);
                if(!canWin(replaced)) return true;
            }
        }
        return false;
    }
}
```

### Improvements

因为计算过程中有些substring可能会重复计算，所以用1个hashMap存计算过的string是canWin，不是canWin，不用重复计算已经计算过的string了。

```java
public class Solution {
    HashMap<String, Boolean> map = new HashMap();
    
    public boolean canWin(String s) {
        if(s.length() < 2) return false;
        if(map.containsKey(s)) return map.get(s);
        
        for(int i = 0; i < s.length() - 1; i ++) {
            if(s.charAt(i) == '+' && s.charAt(i+1) == '+') {
                String replaced = s.substring(0, i) + "--" + s.substring(i+2);
                if(!canWin(replaced)) {
                    map.put(s, true);
                    return true;
                }
            }
        }
        map.put(s, false);
        return false;
    }
}
```



