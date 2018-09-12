# 231. Power of Two

> Given an integer, write a function to determine if it is a power of two.

### 思路 1\)

一开始想到的思路就是如果1个数X是2的N次方，那么X % 2的remainder肯定是0，然后X / 2 后再 % 2的remainder肯定还是0，直到X的值变为2。有点特殊的就是2的零次方是1，肯定单独拿出来当作special case。反之，如果中间某次的余数不为零，那么说明不是power of 2。

### Code 1\)

```java
public class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n == 1) return true;
        while (n > 2) {
            if (n % 2 != 0) return false;
            n = n / 2;
        }
        return n == 2;
    }
}
```

### 思路 2\)

如果是power of 2，那么该integer的bit只有可能有1个1，其他都是0。

* 可以用 **Integer.bitCount\(\)** 来计算
* 也可以用 trick ** n & \(n-1\) == 0 ** 来看n的bits上是不是只有1个1

### Code 2\)

```java
public class Solution {
    public boolean isPowerOfTwo(int n) {
        return n>0 && Integer.bitCount(n) == 1;
    }
}
```

```java
public class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n < 1) return false;
        return (n & (n-1)) == 0;
    }
}
```



