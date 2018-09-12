# 477. Total Hamming Distance

> The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.
>
> Now your job is to find the total Hamming distance between all pairs of the given numbers.
>
> **Example:**
>
> ```
> Input: 4, 14, 2
>
> Output: 6
>
> Explanation: In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
> showing the four bits relevant in this case). So the answer will be:
> HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.
> ```
>
> **Note:**
>
> 1. Elements of the given array are in the range of 0 to 10^9
> 2. Length of the array will not exceed 10^4

### 思路 1\)

暴力解法，两两比较，分别计算每个pair的hamming distance，然后相加。

### Code 1\)

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int rst = 0;
        for(int i = 0; i < nums.length; i ++) {
            for (int j = i + 1; j < nums.length; j ++) {
                rst += Integer.bitCount(nums[i] ^ nums[j]);
            }
        }
        return rst;
    }
}
```

### 思路 2）

上面暴力解法是O\(n^2\)复杂度，太高。看有没有更好方法。那么先观察每个num的同一位bit，发现有些是0，有些是1。比如一共有a个0，b个1，那么对于这一位而言，增加 total hamming distance 是多少，其实就是 a \* b。由此便可以遍历int的32位bit，分别看每一位bit有多少0，多少1，计算求解。

### Code 2\)

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int rst = 0;
        for(int i = 0; i < 32; i ++) {
            int count = 0; // count for bit 1 on last bit of integer
            for(int num : nums) {
                count += (num >> i) & 1;
            }
            rst += count * (nums.length - count);
        }
        return rst;
    }
}
```



