# 681. Next Closest Time

> Given a time represented in the format "HH:MM", form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.
>
> You may assume the given input string is always valid. For example, "01:34", "12:09" are all valid. "1:34", "12:9" are all invalid.
>
> **Example 1:**
>
> ```
> Input: "19:34"
> Output: "19:39"
> Explanation: The next closest time choosing from digits 1, 9, 3, 4, is 19:39, which occurs 5 minutes later.  It is not 19:33, because this occurs 23 hours and 59 minutes later.
> ```
>
> **Example 2:**
>
> ```
> Input: "23:59"
> Output: "22:22"
> Explanation: The next closest time choosing from digits 2, 3, 5, 9, is 22:22. It may be assumed that the returned time is next day's time since it is smaller than the input time numerically.
> ```

### 思路

1. 思路没什么特别的，就是“演绎法”。想怎么才能得到比当前时间大的最小时间。因为需要“尽可能小”，所以从时间string的末尾往前面scan，看每个刻度是否可能变化，因为最开头是更significant的位置。
   1. scan的时候，需要看能否把当前刻度的digit是否可以变大一些，当然这里需要reuse input已经有的digit。如果可以变化的话，则变化的位置的左边所有位置保持不变即可。
   2. 还有一个需要注意，就是timestamp string 中每个digit位置的范围limit不同，比如第一位只能是'0', '1', '2'; 最后一位可以是‘0’-‘9’
   3. 注意在发现某个digit的值不能变大的时候，应该取其可能中的最小digit，而不是保持原样。这样在其左边的digit变大后，得到的新的timestamp 才是尽可能小的。如果所有digit都不可能变大，则说明所要找的时间在“下一天”，那么每一位都是尽可能小的digit也是符合要求的“最小” timestamp。

### Code

```java
class Solution {
    public String nextClosestTime(String time) {
        HashSet<Character> digits = new HashSet();
        for(int i = 0; i < 5; i ++) {
            if(i != 2) digits.add(time.charAt(i));
        }
        
        boolean increased = false; // means if one of digit changed to closest bigger one
        StringBuilder rst = new StringBuilder();
        
        for(int i = 4; i >= 0; --i) { // insert char to result from back to begin
            char tmp = time.charAt(i);
            
            if(i != 2 && !increased) {
                // limit is upper bound of char value we can insert to result for current position
                char limit = '9';
                if(i == 3) limit = '5';
                else if (i == 1) limit = time.charAt(0) == '2' ? '3' : '9';
                else if (i == 0) limit = '2';
                
                if(tmp < limit) {
                    tmp ++;
                    for(char c = tmp; c <= limit; c ++) { // add first valid bigger char value
                        if(digits.contains(c)) {
                            rst.insert(0, c);
                            increased = true;
                            break;
                        }
                    }
                }
                
                // append smallest char if not changed
                if(!increased) {
                    for(char c = '0'; c <= '9'; c ++) {
                        if(digits.contains(c)) {
                            rst.insert(0, c);
                            break;
                        }
                    }
                }
            } else rst.insert(0, tmp);
        }
        return rst.toString();
    }
}
```



