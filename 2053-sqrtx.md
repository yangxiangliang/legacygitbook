# 69. Sqrt\(x\)

> Implement `int sqrt(int x)`.
>
> Compute and return the square root of _x_, where _x_ is guaranteed to be a non-negative integer.
>
> Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.
>
> **Example 1:**
>
> ```
> Input: 4
> Output: 2
> ```
>
> **Example 2：**
>
> ```
> Input: 8
> Output: 2
> Explanation: The square root of 8 is 2.82842..., and since 
>              the decimal part is truncated, 2 is returned.
> ```

### 思路

二分法

**注意点是这里二分方法的写法，不要造成死循环。然后还有计算过程中超过 integer 界限的情况。**

### Code

```java
class Solution {
    public int mySqrt(int x) {
        int start = 0;
        int end = x;
        while(start <= end) {
            int mid = start + (end - start) / 2;

            // 注意 (mid * mid)可能超越integer的界限，相当于(mid*mid) > x 的情况
            if((long) mid * mid > Integer.MAX_VALUE) end = mid - 1;
            else {
                int tmp = mid * mid;
                if(tmp == x) return mid;
                // 注意这里不要 start = mid，因为这样的话，在start == end - 1的时候，mid 一直等于 start, 会infinite loop
                // 因为 start = mid + 1，可能(start * start) > x的，所以最后不能return start，需要return end
                // end 肯定是在 范围内的
                else if(tmp < x) start = mid + 1;
                else end = mid - 1;
            }
        }
        return end;
    }
}
```



