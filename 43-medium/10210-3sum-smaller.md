# 259. 3Sum Smaller

> Given an array of n integers _nums_ and a _target_, find the number of index triplets `i, j, k`with`0 <= i < j < k < n`that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.
>
> For example, given _nums_=`[-2, 0, 1, 3]`, and _target_ = 2.
>
> Return 2. Because there are two triplets which sums are less than 2: \[-2, 0, 1\], \[-2, 0, 3\]
>
> **Follow up: **
>
> Could you solve it in _O\(n^2\)_ runtime?

### 思路

Brute force的方法的running time是O\(n^3\)，需要到达O\(n^2\)，必须要先sort原来的nums，然后再查找triplets。sort了nums后，从index == 0开始，假定triplets第一个元素取 nums\[i\]，那么我们从 i+1 到 nums.length - 1，bound中找triplets中的另外2个元素。假设left = i+1, right = nums.length - 1，如果nums\[left\] + nums\[right\] &lt; target - nums\[i\], 那么\(left, left +1\), \(left, left +2\), \(left, right\) 都符合条件，然后left++，再来寻找符合条件的元素。反之，则需要减少right bound，即right--， 来看是否满足nums\[left\] + nums\[right\] &lt; target - nums\[i\]的条件。直到left 和 right指针相遇。

### Code

```java
public class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int count = 0;
        for(int i = 0; i < nums.length - 2; i ++) {
            int tmpTarget = target - nums[i];
            int start = i + 1;
            int end = nums.length - 1;
            while(start < end) {
                if(nums[start] + nums[end] < tmpTarget) {
                    count += end - start;
                    start ++;
                } else end --;
            }
        }
        return count;
    }
}
```



