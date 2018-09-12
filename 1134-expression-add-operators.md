# 282. Expression Add Operators

> Given a string that contains only digits `0-9`and a target value, return all possibilities to add **binary **operators \(not unary\) `+`, `-`, or`*`between the digits so they evaluate to the target value.
>
> **Examples:**
>
> ```
> "123", 6 -> ["1+2+3", "1*2*3"] 
> "232", 8 -> ["2*3+2", "2+3*2"]
> "105", 5 -> ["1*0+5","10-5"]
> "00", 0 -> ["0+0", "0-0", "0*0"]
> "3456237490", 9191 -> []
> ```

### 思路

1. 这个题目第一眼看容易没有思路，感觉很多可能，很复杂。仔细想一想，因为每2个或几个digit之间插入什么符号都有可能，只能一个一个尝试，然后沿着path往下走，最后得到target值的path加入结果当中。这个思路是典型的"backtracking"思想。
2. 接下来要研究下怎么写"backtracking"中的recursion函数，首先除了path string和存结果的List&lt;String&gt;必须parse到下一层函数中。
   1. 还需要parse当前path中计算所得的value，用于最后和target比较。
   2. 当然要parse目前遍历到input string的哪一个index了，表示从该index之后开始scan input string。
   3. ** 最关键的是乘号的处理**，因为乘号有优先于+，- 符号的功能。比如一开始的path是“2+3”，那么此时value等于5，如果parse到下一层函数中，需要"2+3\*3"，那么不是value \* 3，而是上面一层得到的那个操作数"3" 乘以 3，此时的value等于减去上一层已经“加”上的操作数”3“，然后再加上 上一层的操作数”3“ 乘以current 层函数的操作数”3“，得到current path的value值。所以还需要记录current path的最后一个操作数到下一层，以防下一层要对其做乘法“\*” 运算。
   4. **需要注意：**有可能好几个digits 组成一个数，当成一个整体来运算，不一定是一个digit就是一个数参与运算。
   5. 题目中没有明确说明，我们需要避免“05”的数字参与运算，因为这样的数字就等于“5”。只把“0”作为单独digit运算，如果遇见多个digits组成的元素，以“0”开头，然后跳过此情况。

### 复杂度

Time: O\(4^N \* N\)，因为每个digit后面可能有4种情况，就是+, -, \* 以及与下一个digit组成新的数字。一共得到\(4^N\)种expression，每一个expression计算需要O\(N\)，所以总的时间复杂度是\(N \* 4^N\)

### Code

```java
class Solution {
    public List<String> addOperators(String num, int target) {
        List<String> rst = new ArrayList();
        helper(num, 0, 0, 0, target, rst, "");
        return rst;
    }
    
    private void helper(String num, int index, long value, long last, int target, List<String> rst, String path) {
        if(index == num.length()) {
            if(value == target) rst.add(path);
            return;
        }
        
        for(int i = index; i < num.length(); i ++) {
            // 注意：parse当前substring组成的integer的时候，该integer可能超出int范围，所以用long表示
            // 同时当前path得到的value值也用long 范围表示
            long cur = Long.parseLong(num.substring(index, i+1));
            if (num.charAt(index) == '0' && i > index) break;
            if(index == 0) helper(num, i+1, cur, cur, target, rst, "" + cur);
            else {
                // + cur
                helper(num, i+1, value + cur, cur, target, rst, path + "+" + cur);
                // - cur
                helper(num, i+1, value - cur, -cur, target, rst, path + "-" + cur);
                // *cur
                helper(num, i+1, value - last + last*cur, last*cur, target, rst, path + "*" + cur);
            }
        }
    }
}
```



