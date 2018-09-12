# 13. Roman to Integer

> Given a roman numeral, convert it to an integer.
>
> Input is guaranteed to be within the range from 1 to 3999.

### 思路

1. 首先弄清楚有哪些Roman 字母，和其对应的数字。{'I','V','X','L','C','D','M'} --&gt; {1,5,10,50,100,500,1000}
2. **\(注意\)** 值得注意的比如是“XI”，表示11，即\(11+1\)。如果是“IX”，即表示的 9 = 10 -1。在 Roman表示中，如果前面一个字母代表的integer a小于后面一个字母代表的integer b，则可看作该2个字母表示的数是\( -a + b\)。

### Code

```java
class Solution {
    public int romanToInt(String s) {
        char[] roman = {'I','V','X','L','C','D','M'};
        int[] num = {1,5,10,50,100,500,1000};
        HashMap<Character, Integer> map = new HashMap();
        for(int i = 0; i < num.length; i ++) map.put(roman[i], num[i]);
        int sum = 0;
        for(int i = 0; i < s.length(); i ++) {
            if(i < s.length() - 1 && map.get(s.charAt(i)) < map.get(s.charAt(i+1))) sum -= map.get(s.charAt(i));
            else sum += map.get(s.charAt(i));
        }
        return sum;
    }
}
```



