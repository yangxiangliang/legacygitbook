# 523. Continuous Subarray Sum

> Given a list of **non-negative **numbers and a target **integer **k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to the multiple of **k**, that is, sums up to n\*k where n is also an **integer**.
>
> **Example 1:**
>
> ```
> Input: [23, 2, 4, 6, 7],  k=6
> Output: True
> Explanation: Because [2, 4] is a continuous subarray of size 2 and sums up to 6.
> ```
>
> **Example 2:**
>
> ```
> Input: [23, 2, 6, 4, 7],  k=6
> Output: True
> Explanation: Because [23, 2, 6, 4, 7] is an continuous subarray of size 5 and sums up to 42.
> ```
>
> **Note:**
>
> 1. The length of the array won't exceed 10,000.
> 2. You may assume the sum of all the numbers is in the range of a signed 32-bit integer.

### 思路 1）

1. 一开始用最简单的方法，就是先用int\[\] sum记录从 nums\[0\] 到 nums\[i\]的所有sum的和。这样便于之后计算从任意nums\[i\] 到 nums\[j\]的sum值。sum\(i, j\) = sums\[j\] - sums\[i-1\] 即可。
2. 然后用另外一个函数判断sum\(i, j\) 是否能被 k 乘除即可。
3. **Corner case 注意： **k 等于 0 的时候，只有被除数也等于0的话，才返回true；只讨论 subarray的size 大于等于2的情况

### 复杂度

O\(N^2\)

### Code 1\)

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        int[] sums = new int[nums.length];
        for(int i = 0; i < nums.length; i ++) {
            if(i == 0) sums[i] = nums[i];
            else sums[i] = nums[i] + sums[i-1];
            if(i >= 1) {
                if(isValid(sums[i], k)) return true;
                // 因为只考虑subarray size 大于1的情况，所以这里j不能取值(i-1)
                for(int j = 0; j < i - 1; j ++) {
                    if(isValid(sums[i] - sums[j], k)) return true;
                }
            }
        }
        return false;
    }

    private boolean isValid(int num, int k) {
        if(k == 0) return num == 0;
        if(num % k == 0) return true;
        return false;
    }
}
```

### 思路 2\)

1. 想办法能不能降低复杂度。这里研究的是余数问题。那么对于nums\[0\] 到 nums\[i\]的sum的结果中能被k整除的部分其实可以不用考虑了。其实我们每次只用相加现在除以k的余数即可。
2. 那么想一想怎么才能 让 nums\[i\] 到 nums\[j\] 的和 能被K乘除。其实相当于是 nums\[0\]到nums\[j\]的和 \(sum\[j\]\) 减去 nums\[0\] 到 nums\[i-1\] 的和 \(sum\[i-1\]\) 能被 k 整除即可。**其实也就是sum\[j\] 除以 k的余数 减去 sum\[i-1\] 除以 k 的余数 等于 0 即可 **。即他们的余数 **相互抵消掉**
3. 所以用一个sum 记录加到目前 nums\[i\]的余数，然后用1个hashMap 记录从最开始的integer加到目前index的余数和index的mapping关系，如果发现hashMap 中已经有相同的余数了，且 index 只差 大于 1，那么就是找到符合要求的sub array了。
4. **这里的思路类似于 Two Sum 用 HashMap 记录的方法**。

### 复杂度

Time: O\(N\)

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap();
        int sum = 0;
        for(int i = 0; i < nums.length; i ++) {
            sum += nums[i];
            if(k != 0) sum %= k;
            // sum 可能自身为0，注意检查 i 大于1，因为subarray size >= 2
            if(sum == 0 && i >= 1) return true;
            if(map.containsKey(sum)) {
                int pre = map.get(sum);
                if(i - pre > 1) return true;
            } else map.put(sum, i);
        }
        return false;
    }
}
```



