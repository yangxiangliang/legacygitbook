# 670. Maximum Swap

> Given a non-negative integer, you could swap two digits **at most **once to get the maximum valued number. Return the maximum valued number you could get.
>
> **Example 1:**
>
> ```
> Input: 2736
> Output: 7236
> Explanation: Swap the number 2 and the number 7.
> ```
>
> **Example 2:**
>
> ```
> Input: 9973
> Output: 9973
> Explanation: No swap.
> ```
>
> **Note:**
>
> 1. The given number is in the range \[0, 10^8\].

### 思路

1. 首先直接想怎么才能swap得到max的value。那么当然就是把最大的1位尽量放到最前面。那么对于最左边的most significant 位来说，如果其右边中最大digit 比 自己大，那么应该最左边digit与其交换即可。如果最左边digit已经是所有digit最大的，那么保持不变即可。接着看从左往右第二位digit，如果该位置digit右边所有数字中最大的比其大，那么与之交换，反之接着往右遍历，直到遍历结束。如果整个digit都是降序排列，那么当然就不用swap了。

### Code

```java
class Solution {
    public int maximumSwap(int num) {
        // rst 存num的各位digit数字，rst.get(0) 表示num的最右边 的digit
        List<Integer> rst = new ArrayList();
        while(num > 0) {
            rst.add(num % 10);
            num = num / 10;
        }
        // max[i]表示rst中在第ith digit左边的最大digit的index，包括ith digit自己
        int[] max = new int[rst.size()];
        for(int i = 0; i < max.length; i ++) {
            if(i == 0) max[0] = 0;
            // 注意：如果这里取值相等的时候，任然传递max[i-1]，而不是max[i] = i，因为尽量把最大digit的index
            // 往rst左边取，即least significant 方向取，这样之后与most significant方向的digit交换时
            // 才能尽量得到最大值
            else if (rst.get(i) <= rst.get(max[i-1])) max[i] = max[i-1];
            else max[i] = i;
        }

        // 因为只能交换一次，所以用swapped记录是否已经swap了
        boolean swapped = false;
        int result = 0;
        // 从最高位往最低位scan，因为尽量交换最高位，才能得到最大值
        for(int i = rst.size() - 1; i >= 0; i --) {
            if(rst.get(i) < rst.get(max[i]) && !swapped) {
                int tmp = rst.get(i);
                rst.set(i, rst.get(max[i]));
                rst.set(max[i], tmp);
                swapped = true;;
            }
            result = 10 * result + rst.get(i);
        }
        return result;
    }
}
```

### Code 2\)

思路和上面一样，写法更简单

```java
class Solution {
    public int maximumSwap(int num) {
        // 把 input num转化成 char array
        char[] chars = Integer.toString(num).toCharArray();
        int[] last = new int[10];
        // last[] 记录[0, 9] 每个digit出现在num中的最右边位置
        for(int i = 0; i < chars.length; i ++) last[chars[i] - '0'] = i;
        
        for(int i = 0; i < chars.length; i ++) {
            // 从最大的digit开始，看有没有该digit出现在i的右边，如果有则和i位置交换即可
            // 如果i右边有好几个相同的比chars[i]大的digit，则取最右边的那个即可，才能让结果最大值
            // 所以上面last[] 记录的是[0, 9] 某一个digit在num中“最右边”的位置
            for(int d = 9; d > chars[i] - '0'; d --) {
                if(last[d] > i) {
                    char tmp = chars[i];
                    chars[i] = chars[last[d]];
                    chars[last[d]] = tmp;
                    return Integer.valueOf(new String(chars));
                }
            }
        }
        return num;
    }
}
```



