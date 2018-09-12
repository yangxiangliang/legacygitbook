# 228. Summary Ranges

> Given a sorted integer array without duplicates, return the summary of its ranges.
>
> For example, given`[0,1,2,4,5,7]`, return`["0->2","4->5","7"].`

### 思路

从index = 0 开始，记录nums\[index\]的值，如果后面一个值只比其大1，则index++，直到这一段range的最大值，然后将这段range加入结果中，then开始找下一段range。注意区别range中只有1个值的情况，不需要“-&gt;”符号。

### Code

```java
public class Solution {
    public List<String> summaryRanges(int[] nums) {
        List<String> rst = new ArrayList();
        int index = 0;
        while(index < nums.length) {
            int left = nums[index];
            while(index < nums.length - 1 && nums[index+1] == nums[index] + 1) index ++;
            if(nums[index] == left) rst.add(left + "");
            else rst.add(left + "->" + nums[index]);
            index ++; // 仔细：别忘了 index ++
        }
        return rst;
    }
}
```



