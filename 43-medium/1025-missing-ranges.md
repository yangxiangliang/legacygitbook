# 163. Missing Ranges

> Given a sorted integer array where **the range of elements are in the inclusive range \[lower, upper\]**, return its missing ranges.
>
> For example, given`[0, 1, 3, 50, 75]`,  lower = 0 and upper = 99, return`["2", "4->49", "51->74", "76->99"].`

### 思路

解题思路容易想到，就是移动lower pointer去找数组中剩下的数的lower bound，然后最后加上数组最后一个数字和upper bound之间的gap。**注意坑:** 就是改变lower pointer的时候可能超过 ** Integer.MAX\_**_**VALUE ，那么不能再增加lower pointer的值，其只能保持在 Integer.MAX\_VALUE**_

### Code 

```java
public class Solution {
    public List<String> findMissingRanges(int[] nums, int lower, int upper) {
        List<String> rst = new ArrayList();
        
        // 注意nums为empty的special case
        if(nums.length == 0) {
            if(lower == upper) rst.add(lower+"");
            else rst.add(lower + "->" + upper);
            return rst;
        }
        
        for(int i = 0; i < nums.length; i ++) {
            
            // 注意lower可能应该到最大值，不能再 + 1
            if(nums[i] == lower) lower = lower == Integer.MAX_VALUE ? Integer.MAX_VALUE : lower+1 ;
            else if(nums[i] > lower) {
                if(nums[i] > lower + 1)  rst.add(lower + "->" + (nums[i] -1));
                else rst.add(lower + "");
                // 注意 nums[i] 已经到最大值的情况
                lower = nums[i] == Integer.MAX_VALUE ? Integer.MAX_VALUE : nums[i]+1;
            }
            
            if(i == nums.length - 1 && upper > nums[i]) {
                if(upper > nums[i] + 1) rst.add(nums[i]+1 + "->" + upper); 
                else rst.add(upper + "");
            }
        }
        return rst;
    }
}
```



