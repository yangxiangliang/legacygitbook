# 202. Happy Number

> Write an algorithm to determine if a number is "happy".
>
> A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 \(where it will stay\), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.
>
> **Example:  **19 is a happy number
>
> * 1^2 + 9^2 = 82
> * 8^2 + 2^2 = 68
> * 6^2 + 8^2 = 100
> * 1^2 + 0^2 + 0^2 = 1

### 思路

1. 就用“演绎”的思路，从一开始求n的每个digit的平方和，然后看平方和是否是1。然后更新n值为新的平方和的值。
2. 因为不是happy number，会进入一个循环，所以用set记录过程中所有的平方和的值，如果遇见重复的且没有遇见1，则return false即可。

### Code 1\)

```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet();
        int next = 0;
        while(true) {
            int a = n % 10;
            next = a * a;
            n = n / 10;
            while(n > 0) {
                a = n % 10;
                next += a * a;
                n = n / 10;
            }
            n = next;
            if(n == 1) return true;
            if(set.contains(n)) break;
            set.add(n);
        }
        return false;
    }
}
```

### Code 2\)

思路一样，写法更organize 一些

```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet();

        while(!set.contains(n)) {
            set.add(n);
            n = getSquareOfAllDigits(n);
        }
        return n == 1;
    }

    private int getSquareOfAllDigits(int n) {
        int rst = 0;
        while(n > 0) {
            int tmp = n % 10;
            rst += tmp * tmp;
            n = n / 10;
        }
        return rst;
    }
}
```



