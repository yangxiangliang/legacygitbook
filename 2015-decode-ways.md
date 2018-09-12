# 91. Decode Ways

> A message containing letters from `A-Z`is being encoded to numbers using the following mapping:
>
> ```
> 'A' -> 1
> 'B' -> 2
> ...
> 'Z' -> 26
> ```
>
> Given an encoded message containing digits, determine the total number of ways to decode it.
>
> For example,  
> Given encoded message`"12"`, it could be decoded as`"AB"`\(1 2\) or`"L"`\(12\).
>
> The number of ways decoding`"12"`is 2.

### 思路

1. “XXX12XXX” 这里找输入的integer string时，有可能出现变化的情况是看input string中第i个character和\(i-1\)个character能否对应到‘A'-- 'Z' 中的字母，如果可以，则有多种decode的方法。如果不行，则decode inputString.substring\(0, i\)的种类个数应该与inputString.substring\(0, i-1\)的种类个数相同；如果可以，则input.substring\(0, i\)的种类个数应该还要加上 input.substring\(0, i-2\)的decode的种类个数。
2. 这里显然input.substring\(0, i\)的decode的种类个数与之前的情况相关，那么自然想到DP来操作，用一个int\[\] rst来记录input的每个substring\(0, i\)的decode的数量，直到遍历完整个input string。
3. **\(坑\) **这里input中可能会有‘0’的情况，因为没有一个单独的字母可以对应‘0’，所以如果inputString.charAt\(i\) == '0', 那么rst\[i\] = 0。如果charAt\(i-1\), charAt\(i\) 可以组成10，或则20，那么rst\[i\] += rst\[i-2\]。并且注意，charAt\(i-1\), charAt\(i\)的情况，如果组成的数字是小于10的，比如“08”， “09”，那么 rst\[i\] 不能加上 rst\[i-2\]。因为没有字母对应“08”， ‘’09"。只能把charAt\(i\)当成单独的字母对应，即 rst\[i\] = rst\[i-1\]

### Code

```java
class Solution {
    public int numDecodings(String s) {
        if(s.length() == 0) return 0;
        int[] rst = new int[s.length() + 1];
        rst[0] = 1; // initialization
        for(int i = 1; i <= s.length(); i ++) {
            if(s.charAt(i-1) != '0') rst[i] = rst[i-1];
            if(i >= 2) {
                int val = Integer.valueOf(s.substring(i-2, i));
                if(val >= 10 && val <= 26) rst[i] += rst[i-2];
            }
        }
        return rst[s.length()];
    }
}
```

### 思路 2）

思路和上面一样，但是可以做到constant space。因为发现DP的时候，当前number结果只和往前数1个，以及往前数2个的值相关。所以我们只用维护这2个值即可，不用一个数组来维护所有值，则可以做到constant space.

### Code 2\)

```java
class Solution {
    public int numDecodings(String s) {
        if(s.length() == 0 || s.charAt(0) == '0') return 0;
        
        // r1就相当于上面方法中的 rst[i-1]值，r2就相当于上面方法中的 rst[i-2]值。
        int r1 = 1, r2 = 1;
        for(int i = 1; i < s.length(); i ++) {
            // 以为后面计算中r1值可能变化，先用pre存下来，现在的r1值就是下一轮循环的r2值。最后要r2赋值为该r1值
            int pre = r1;
            
            // 更新r1值
            if(s.charAt(i) == '0') {
                if(s.charAt(i - 1) == '1' || s.charAt(i - 1) == '2') r1 = r2;
                else r1 = 0;
            } else if(s.charAt(i - 1) == '1' || s.charAt(i - 1) == '2' && s.charAt(i) <= '6') r1 += r2;
            // 进入下一轮循环的r2值就是该循环开始时候的r1值
            r2 = pre;
        }
        return r1;
    }
}
```



