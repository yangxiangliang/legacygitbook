# 334. Increasing Triplet Subsequence

> Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.
>
> Formally the function should:
>
> ```
> Return true if there exists i, j, k 
> such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
> ```
>
> Your algorithm should run in O\(n\) time complexity and O\(1\) space complexity.
>
> **Example:**
>
> Given`[1, 2, 3, 4, 5]`,  
> return`true`.
>
> Given`[5, 4, 3, 2, 1]`,  
> return`false`.

### 思路

1. **注意： 这里的i, j, k 不用是连续的3个值，这一点容易搞错。**
2. 因为需要找到3个数是递增的，一开始想怎么最容易实现这个结果。那么应该这3个最左边的那个数尽量小的min，然后中间这个数是比min大的当中最小的，即可以看做“second smallest”，那么只用继续往右边遍历，找到一个数比“second smallest” 大的就可以了。

### Code

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        // target means second smallest number for left of array
        // we just need to find one more num which is bigger than target in rest right of array
        int target = Integer.MAX_VALUE;
        int min = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i ++) {
            if(nums[i] > target) return true;
            // 注意target即 second smallest，需要取的是target可取值当中的最小，这样才可以尽可能找到nums[i] > target的情况
            if(nums[i] > min) {
                target = Math.min(target, nums[i]);
            }
            min = Math.min(min, nums[i]);
        }
        return false;
    }
}
```



