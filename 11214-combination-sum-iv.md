# 377. Combination Sum IV

> Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.
>
> **Example:**
>
> ```
> nums = [1, 2, 3]
> target = 4
>
> The possible combination ways are:
> (1, 1, 1, 1)
> (1, 1, 2)
> (1, 2, 1)
> (1, 3)
> (2, 1, 1)
> (2, 2)
> (3, 1)
>
> Note that different sequences are counted as different combinations.
>
> Therefore the output is 7.
> ```
>
> **Follow up:**
>
> What if negative numbers are allowed in the given array?
>
> How does it change the problem?
>
> What limitation we need to add to the question to allow negative numbers?

### 思路

1. combination sum的时候，因为不知道后面的数是什么，只能一个一个往下走，类似于backtracking。但是有个小trick，因为数组中都是positive integer，那么如果每一层往下的需要，当前需要sum的结果是负数，则可以直接返回，因为都是positive的话，最后结果不可能是负数。如果当前需要的sum的结果刚好是0，说明找到了合适的组合。
2. 还有问题需要注意，就是这里不同顺序的sequence是当做不能的combinations，而且遍历的时候没有前后顺序。比如例子中，可以先取个1，再取个3，然后又可以回来取个1。那么就不同于其他的题目，有时候要parse一个start index进入函数，表示当前这一层recursion，可以从哪一个元素开始往后取值，这里不用，因为每一层recursion都有可能从头开始取值。每一层需要遍历所有数组里面的情况即可。
3. “为了加快速度， 防止重复计算，用一个HashMap 记录target 值和其对应的情况数量即可”

### Follow Up

如果数组当中有negative的数，那么这个结果可能有无穷循环的list，因为每个元素可以重复取值。比如\[1, -1\]，它们可以反复取值，一直等于0，那么这个结果中list就可以无穷长。所以有negative的情况下，需要限制最后sequence输出的长度，才可以。所以recursion 时候，需要parse 当前这个sequence的长度了，如果长度超出限制了，那么就返回，不继续往下遍历了。

### Code

```java
class Solution {
    HashMap<Integer, Integer> map = new HashMap();

    public int combinationSum4(int[] nums, int target) {
        return helper(nums, target);
    }

    private int helper(int[] nums, int target) {
        if(map.containsKey(target)) return map.get(target);
        int rst = 0;
        if(target <= 0 ) {
            if(target == 0) rst = 1;
            map.put(target, rst);
            return rst;
        }
        for(int i = 0; i < nums.length; i ++) rst += helper(nums, target - nums[i]);
        map.put(target, rst);
        return rst;
    }
}
```



