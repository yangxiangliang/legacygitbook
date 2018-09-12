# 238. Product of Array Except Self

> Given an array of n integers where n&gt;1,`nums`, return an array`output`such that `output[i]`is equal to the product of all the elements of`nums`except`nums[i]`.
>
> Solve it **without division **and in O\(n\).
>
> For example, given`[1,2,3,4]`, return`[24,12,8,6]`.
>
> **Follow up:**
>
> Could you solve it with constant space complexity? \(Note: The output array **does not **count as extra space for the purpose of space complexity analysis.\)

### 思路

最简单的思路就是用2个数组，一个int\[\] left, int\[\] right。left\[i\] 表示i 左边所有元素的product，right\[i\] 表示i 右边所有元素的product。

有上面2个int\[\]后，容易求得所需结果。

### Code

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] left = new int[nums.length];
        int[] right = new int[nums.length];
        for(int i = 0; i < nums.length; i ++) {
            if(i == 0) {
                left[0] = 1;
                right[nums.length - 1] = 1;
            }
            else {
                left[i] = left[i-1] * nums[i-1];
                right[nums.length - 1 - i] = right[nums.length - i] * nums[nums.length - i];
            }
        }
        int[] rst = new int[nums.length];
        for(int i = 0; i < nums.length; i ++) {
            rst[i] = left[i] * right[i];
        }
        return rst;
    }
}
```

### Follow up

不允许用除了结果意外的extra space。那么就思考上面几个int\[\]可不可以合并成一个结果的int\[\]。那么int\[\] left, int\[\] right只能用1个，想从左边到右边的时候，可以同样方法计算int\[\] left。然后从nums\[\] 的右边scan到左边，可以只用一个var记录当前右边所有元素的product，那么left\[i\] = left\[i\] \* var。而且因为第二次scan是从右边到左边，计算left\[i\]的时候，left\[i\]的值没有改变，就是表示i左边所有元素的product。这样left\[\] 最后就是所需要的结果。

### Code 2\)

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] rst = new int[nums.length];
        for(int i = 0; i < nums.length; i ++) {
            if(i == 0) rst[0] = 1;
            else rst[i] = rst[i-1] * nums[i-1];
        }
        int right = 1;
        for(int i = nums.length - 2; i >= 0; i --) {
            right = right * nums[i+1];
            rst[i] = rst[i] * right;
        }
        return rst;
    }
}
```





