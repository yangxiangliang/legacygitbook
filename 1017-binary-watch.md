# 401. Binary Watch

> A binary watch has 4 LEDs on the top which represent the **hours**\(**0-11**\), and the 6 LEDs on the bottom represent the **minutes**\(**0-59**\).
>
> Each LED represents a zero or one, with the least significant bit on the right.
>
> Given a non-negative integer \_n \_which represents the number of LEDs that are currently on, return all possible times the watch could represent.
>
> **Example:**
>
> ```
> Input: n = 1
> Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
> ```
>
> **Note:**
>
> * The order of output does not matter.
> * The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
> * The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

### 思路

将表面想象成一个数组 \[1, 2, 4, 8, 1, 2, 4, 8, 16, 32\] 前面4个表示hour的，后面6个表示minute的。然后用helper function作backtracking，用 index表示到该数组的位置，到数组的时候，有2种选择，点亮该index对应的位置，或则不点亮。然后用1个参数 int remains 记录还剩下几个灯需要点亮，当remains == 0的时候，便可以把valid的结果加入list中。

### Code

```java
public class Solution {
    public List<String> readBinaryWatch(int num) {
        List<String> rst = new ArrayList();
        build(rst, num, 0, 0, 0);
        return rst;
    }
    
    public void build(List<String> rst, int remains, int hour, int minute, int index) {
        if(remains == 0) {
            if(hour <= 11 && minute <= 59) rst.add(hour + ":" + (minute < 10 ? "0" + minute : minute));
            return;
        }
        // 如果 remains + index > 10，表示就算点亮所有之后的灯，也不可能让 remains = 0
        if(remains + index > 10) return;
        
        if(index <= 3) build(rst, remains - 1, hour + (1 << index), minute, index + 1); // 点亮该hour的位置
        if(index > 3) build(rst, remains - 1, hour, minute + (1 << (index-4)), index + 1); // 点亮该minute的位置
        build(rst, remains, hour, minute, index + 1); // 不点亮该位置
    }
}
```



