# 204. Count Primes

> Count the number of prime numbers less than a non-negative number, **n**.
>
> **Example:**
>
> ```
> Input: 10
> Output: 4
> Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
> ```

### 思路

1. 基本思路[参考这里](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)。基本思想就是从2开始，用已知的prime标记处小于n的所有no primes的数字，如此可以标记处所有小于的非prime的数。

### Code

```java
class Solution {
    public int countPrimes(int n) {
        boolean[] noPrimes = new boolean[n];
        int count = 0;
        for(int i = 2; i < n; i ++) {
            if(!noPrimes[i]) {
                count ++;
                for(int j = 2; j * i < n; j ++) {
                    noPrimes[j * i] = true;
                }
            }
        }
        return count;
    }
}
```



