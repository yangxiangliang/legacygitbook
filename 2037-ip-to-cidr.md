# 751. IP to CIDR

> Given a start IP address`ip`and a number of ips we need to cover`n`, return a representation of the range as a list \(of smallest possible length\) of CIDR blocks.
>
> A CIDR block is a string consisting of an IP, followed by a slash, and then the prefix length. For example: "123.45.67.89/20". That prefix length "20" represents the number of common prefix bits in the specified range.
>
> **Example 1:**
>
> ```
> Input: ip = "255.0.0.7", n = 10
> Output: ["255.0.0.7/32","255.0.0.8/29","255.0.0.16/32"]
> Explanation:
> The initial ip address, when converted to binary, looks like this (spaces added for clarity):
> 255.0.0.7 -> 11111111 00000000 00000000 00000111
> The address "255.0.0.7/32" specifies all addresses with a common prefix of 32 bits to the given address,
> ie. just this one address.
>
> The address "255.0.0.8/29" specifies all addresses with a common prefix of 29 bits to the given address:
> 255.0.0.8 -> 11111111 00000000 00000000 00001000
> Addresses with common prefix of 29 bits are:
> 11111111 00000000 00000000 00001000
> 11111111 00000000 00000000 00001001
> 11111111 00000000 00000000 00001010
> 11111111 00000000 00000000 00001011
> 11111111 00000000 00000000 00001100
> 11111111 00000000 00000000 00001101
> 11111111 00000000 00000000 00001110
> 11111111 00000000 00000000 00001111
>
> The address "255.0.0.16/32" specifies all addresses with a common prefix of 32 bits to the given address,
> ie. just 11111111 00000000 00000000 00010000.
>
> In total, the answer specifies the range of 10 ips starting with the address 255.0.0.7 .
>
> There were other representations, such as:
> ["255.0.0.7/32","255.0.0.8/30", "255.0.0.12/30", "255.0.0.16/32"],
> but our answer was the shortest possible.
>
> Also note that a representation beginning with say, "255.0.0.7/30" would be incorrect,
> because it includes addresses like 255.0.0.4 = 11111111 00000000 00000000 00000100 
> that are outside the specified range.
> ```
>
> **Note:**
>
> 1. ip will be a valid IPv4 address.
> 2. Every implied address \(ip + x\) \(for x &lt; n\) will be a valid IPv4 address.
> 3. n will be an integer in the range \[1, 1000\].

### 思路

1. 思路比较容易，就按照题目意思“演绎”法正常往下想即可。一开始把ip转化成整数的时候，主要溢出的问题，因为ip string是32位，最左边的一位不是用来表示符号的，所以可以表示的最大值 2^32 - 1 超出的int的范围 2^31。所以把IP string转化为整数的时候，用long表示。
2. 这里用 x 表示一开始IP string 转化成的long值。比如 x 是 11111111 00000000 00000000 00001000，那么求x能表示的IP address的个数，需要求x的lowest 1 bit的位置，比如这里 lowest 1 bit的位置在从右到左第4个。从lowest 1 bit开始的左边所有bit相同的情况下\(即前29 bit相同的情况下\)，可以表示 1000 = 8 个不同的IP address。
3. **Trick**：这里怎么求x的lowest 1 bit 的位置的值。需要用到反码和补码的问题。\(-x\) 表示 x的反码 + 1，那么 x & \(-x\) 结果就是 1000 = 8，可以得到lowest 1 bit的信息。该值也就是以 lowest 1 bit 以及其左边所有bit 作为prefix，可以表示的address的个数maxCover。

### Code

```java
class Solution {
    public List<String> ipToCIDR(String ip, int n) {
        long ipLong = 0;
        for(String str : ip.split("\\.")) {
            ipLong = ipLong * 256 + Integer.valueOf(str);
        }
        
        List<String> rst = new ArrayList<>();
        while(n > 0) {
            long maxCover = ipLong & (-ipLong);
            while(maxCover > n) maxCover /= 2;
            rst.add(longToIP(ipLong, maxCover));
            ipLong += maxCover;
            n -= maxCover;
        }
        return rst;
    }
    
    private String longToIP(long x, long coverNum) {
        int len = 33;
        while(coverNum > 0) {
            coverNum /= 2;
            len --;
        }
        return (x >> 24) % 256 + "." + (x >> 16) % 256 + "." + (x >> 8) % 256 + "." + x % 256 + "/" + len;
    }
}
```



