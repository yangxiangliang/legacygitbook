# 273. Integer to English Words

> Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 2^31 - 1.
>
> **Example**
>
> ```
> 123 -> "One Hundred Twenty Three"
> 12345 -> "Twelve Thousand Three Hundred Forty Five"
> 1234567 -> "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
> ```

### 思路

1. 题目思路很容易，就是正面推理，“演绎法”的感觉。因为输入最多 2^31 - 1，是2 billions，所以结果最多是到billion级别。想一想平时怎么把一个数字转化为English words的，因为English words是按照1000一个刻度来划分的，即XXX Billion, XXX Million, XXX Thousand.... 所以想到可以用1个函数专门转化小于1000的number成English words，这样分别转化Billion的个数，Million的个数， Thousand的个数，最多加上小于Thousand的English words。
2. 这里还有个关键是怎么构造integer to english words 的mapping，用HashMap当然可以，但是比较麻烦。可以考虑直接用String\[\] str\_arry = {"", "One", "Two", .....}，这里将index 与 数字相对应，即index 1 对应“One”，这样便于后面计算。然后因为1-19 都是有自己的English words，所以前面array 里面包括一直到19的words。同样的方法表示出“Ten”, "Twenty",..... 等mapping

### Code

```java
class Solution {
    private final String[] LESS_THAN_20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    private final String[] TENS = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    private final String[] THOUSANDS = {"", "Thousand", "Million", "Billion"};

    public String numberToWords(int num) {
        if(num == 0) return "Zero";

        String rst = "";
        int i = 0;
        while(num > 0) {
            // 注意判断 num % 1000 是否为0，如果为0，子函数范围“”，还加上THOUSANDS里面的字母的话，就构造错误
            // 比如 1000000，除以1000后，num = 1000, num % 1000 == 0,那么结果应该是"One Million"，不包含“Thousand” word
            if(num % 1000 != 0) rst = smallerThanThousand(num % 1000) + THOUSANDS[i] + " " + rst;
            num = num / 1000;
            i++;
        }
        return rst.trim(); // 关键：用trim()去掉首尾的space，这样可以少考虑中间的很多Corner case
    }

    private String smallerThanThousand(int num) {
        if(num == 0) return "";
        if(num < 20) return LESS_THAN_20[num] + " ";
        // 这里用recursion写最方便
        if(num < 100) return TENS[num / 10] + " " + smallerThanThousand(num % 10);
        return LESS_THAN_20[num / 100] + " Hundred " + smallerThanThousand(num % 100);
    }
}
```



