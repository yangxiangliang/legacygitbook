# 398. Random Pick Index

> Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.
>
> **Note:**
>
> The array size can be very large. Solution that uses too much extra space will not pass the judge.
>
> ```
> int[] nums = new int[] {1,2,3,3,3};
> Solution solution = new Solution(nums);
>
> // pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
> solution.pick(3);
>
> // pick(1) should return 0. Since in the array only nums[0] is equal to 1.
> solution.pick(1);
> ```

### 思路

因为说了输入size可能很大，其实就是可能是个stream data，不能用太多extra space。

这里具体套路就是 “水塘检验” 方法，和 [10.2.50 Linked List Random Node ](/43-medium/10250-linked-list-random-node.md) 里面思路一样。

### Code

```java
class Solution {

    int[] samples;
    Random random = new Random();
    public Solution(int[] nums) {
        samples = nums;
        random = new Random();
    }
    
    public int pick(int target) {
        int count = 0; // record how many elements already found equals target
        int rst = 0;
        for(int i = 0; i < samples.length; i ++) {
            if(target == samples[i]) {
                count ++;
                if(random.nextInt(count) == 0) rst = i;
            }
        }
        return rst;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int param_1 = obj.pick(target);
 */
```



