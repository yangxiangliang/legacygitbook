# 393. UTF-8 Validation

> A character in UTF8 can be from **1 to 4 bytes **long, subjected to the following rules:
>
> 1. For 1-byte character, the first bit is a 0, followed by its unicode code.
> 2. For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.
>
> Given an array of integers representing the data, return whether it is a valid utf-8 encoding.
>
> **Note:**
>
> The input is an array of integers. Only the **least significant 8 bits **of each integer is used to store the data. This means each integer represents only 1 byte of data.
>
> **Example 1: **
>
> ```
> data = [197, 130, 1], which represents the octet sequence: 11000101 10000010 00000001.
>
> Return true.
> It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.
> ```
>
> **Example 2:**
>
> ```
> data = [235, 140, 4], which represented the octet sequence: 11101011 10001100 00000100.
>
> Return false.
> The first 3 bits are all one's and the 4th bit is 0 means it is a 3-bytes character.
> The next byte is a continuation byte which starts with 10 and that's correct.
> But the second continuation byte does not start with 10, so it is invalid.
> ```

### 思路

根据题目意思，从第一个num开始看，其most significant bit从左到右有几个1，如果是0，说明是1byte，则可以skip过。如果有2，3，4个1，则往后查following number是不是最左边2位是10。

**注意Take away**:  **检查num 是不是 10000000 \(128\)，则用一个 11000000 \(192\) 和它 & 一下，如果结果是 10000000 \(128\)，则符合要求。**

### Code

```java
public class Solution {
    public boolean validUtf8(int[] data) {
        for(int i = 0; i < data.length; i ++) {
            int count = getBytesNum(data[i]);
            if(count == -1) return false; // 说明data[i]本来是invalid的
            while(count > 1) {
                i ++;
                count --;
                // 看 data[i]是不是 xxx10000000的形式
                // 192 是 11000000，如果data[i] 是xxx10000000那么 &后一定是 10000000，即128
                if(i < data.length && (data[i] & 192) == 128) continue;
                else return false;
            }
        }
        return true;
    }

    public int getBytesNum(int num) {
        int move = 7;
        while(move >= 0 && ((num >> move) & 1) == 1) move --;
        int count = 7 - move;
        // count不为1，因为如果是1 byte，最左边1位是0，因为最多为4 bytes，所以count也不会大于4
        if(count == 1 || count > 4) return -1;
        return count;
    }
}
```



