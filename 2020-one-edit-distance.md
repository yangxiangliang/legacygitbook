### 161. One Edit Distance

> Given two strings S and T, determine if they are both one edit distance apart.

### 思路

1. 对于这个题目一开始容易想复杂，容易想到用DP，开辟一个2D array boolean\[i\]\[j\]来传递DP结果。先看boolean\[i\]\[j\] 是否是one edit distance, 再看boolean\[i+1\]\[j\], boolean\[i\]\[j+1\] 是否是one edit distance。
2. 其实不用，仔细想想什么情况下，才可能让S和T是one edit distance。
   1. 首先S和T的长度必须相等或则相差1
   2. 如果S和T的长度相等，那么S和T中必有一个位置对应的character是不同的。**但是该character的前半段和后半段对应的substring应该都是相等的。**
   3. 如果S和T的长度不同，比如S短一些，那么等于是T多了一个character。**T中多的这个character的前半段和后半段对应的substring和S中对应的也必须是相等的。**
3. 想清楚上面的结果，便可以将S和T对应位置的character一个与一个比较，比较到有不同的时候，则分以上几种情况处理即可。如果全部都相同，则看S,T的长度是否一样，如果一样，表示2个string相同，return false；如果长度相差1，则return true。

### Code

```java
class Solution {
    public boolean isOneEditDistance(String s, String t) {
        int n = s.length();
        int m = t.length();
        // limit 取(m, n)中的较小值
        for(int i = 0; i < Math.min(m, n); i ++) {
            if(s.charAt(i) == t.charAt(i)) continue;
            // i 之前的substring已经比较过了，肯定是相同的，所以只比较后面的substring即可
            if(n == m) return s.substring(i+1).equals(t.substring(i+1));
            else if(n < m) return s.substring(i).equals(t.substring(i+1));
            else if(n > m) return s.substring(i+1).equals(t.substring(i));
        }
        // 此时说明比较的char都相同，只有长度差1的时候才会return true
        return Math.abs(n-m) == 1;
    }
}
```



