# 158. Read N Characters Given Read4 II - Call multiple times

> The API:`int read4(char *buf)`reads 4 characters at a time from a file.
>
> The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.
>
> By using the`read4`API, implement the function`int read(char *buf, int n)`that readsncharacters from the file.
>
> **Note:**
>
> The `read`function may be called multiple times.

### 思路

这个题目类似题目[157](https://leetcode.com/problems/read-n-characters-given-read4/description/)，但是这个题目在1个test中可能被call multiple times。多次被call的问题在于，比如第一次call只需要3个char，但是return了4个char，那么需要把另外1个剩下的char存在class中，然后第二次call的时候先return 上次剩下的char，然后再继续read新的char。

### Code 1\)

code比较冗余的写法，但是思路比较好理解

```java
/* The read4 API is defined in the parent class Reader4.
      int read4(char[] buf); */

public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    int remains = 0;
    List<Character> list = new ArrayList(); // 存上次call后remain的character

    public int read(char[] buf, int n) {
        int total = 0;
        if(n <= list.size()) {
            for(int i = 0; i < n; i ++) {
                buf[i] = list.get(0);
                list.remove(0); // 从list read出1个后，需要将其从list中remove掉
                total++;
            }
            return total;
        }

        n = n - list.size();
        int size = list.size();
        for(int i = 0; i < size;  i ++) {
            buf[i] = list.get(0);
            list.remove(0);
            total++;
        }

        for(int i = 0; i < n / 4; i ++) {
            char[] tmp = new char[4];
            int count = read4(tmp);

            for(int k = 0; k < count; k ++) buf[total++] = tmp[k];
            if(count < 4) return total;
        }
        n = n % 4;
        char[] tmp = new char[4];
        int count = read4(tmp);
        for(int i = 0; i < Math.min(n, count); i ++) buf[total++] = tmp[i];
        // 如果 n < count，说明有read出的char没有全部在该次call中return，需要加入记录remain character的list中
        for(int i = n; i < count; i ++) list.add(tmp[i]);
        return total;
    }
}
```

### Code 2\)

简洁的写法，用buffCount记录当前read出来的char的数量，buffPointer表示应该从buffer的哪个位置开始return 结果。如果buffPointer == 0，那么要重新read4\(\)，直到buffPointer读完整个buffCount后又重新reset buffPointer为0。

```java
/* The read4 API is defined in the parent class Reader4.
      int read4(char[] buf); */

public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    int buffPointer = 0;
    int buffCount = 0;
    char[] buffer = new char[4];

    public int read(char[] buf, int n) {
        int total = 0;
        while(total < n) {
            if(buffPointer == 0) buffCount = read4(buffer);
            if(buffCount == 0) break; // 说明已经读完所有的char了
            while(total < n && buffPointer < buffCount) buf[total++] = buffer[buffPointer++];
            // reset buffPointer = 0，下一次重新read4()新的4个char
            if(buffPointer == buffCount) buffPointer = 0;
        }
        return total;
    }
}
```



