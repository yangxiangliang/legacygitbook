# 340. Longest Substring with At Most K Distinct Characters

> Given a string, find the length of the longest substring T that contains at most k distinct characters.
>
> For example, Given s =`“eceba” `and k = 2,  T is "ece" which its length is 3.

### 思路

1. **Sliding window， 类似的Substring和SubArray的题目时，常用的方法可以考虑 Sliding window，即一个长度的window在string或则array上面滑动。关键就是维护该window的 leftBound和rightBound。**
2. 最开始，left和right  bound都是0，right一直向右，然后记录遇见的char和同一种char的个数，以及distinct char的个数，当distinct char的个数&lt;=K时，都可以update result = rightBound - leftBound + 1。当distinct char的个数大于K时，需要滑动leftBound直到去掉一个distinct char。

### Code 1\)

```java
public class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        HashMap<Character, Integer> map = new HashMap();
        int max = 0;
        int leftBound = 0;
        
        // i 是 right bound
        for(int i = 0; i < s.length(); i ++) {
            char tmp = s.charAt(i);
            map.putIfAbsent(tmp, 0);
            map.put(tmp, map.get(tmp) + 1);
            
            while(map.size() > k) {
                // move left bound
                char left = s.charAt(leftBound++);
                map.put(left, map.get(left) - 1);
                if(map.get(left) == 0) map.remove(left);
            }
            max = Math.max(max, i - leftBound + 1);
        }
        return max;
    }
}
```

Time complexity: 这个题目因为for中有while，所以容易当成O\(n^2\)，其实complexity应该是O\(n\)，n是string的长度。**因为每个char最多被add和remove一次（注意Sliding window类似的解法的complexity可如此分析）**。

### Code 2\)  

**用上面的HashMap 效率不够高，如果题目输入的S是ASCII，则可以用 int\[\] map = new int\[256\]  代替HashMap**

**Trick 注意：常用 int\[\] map = new int\[256\] 来记录character。**

```java
public class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        int[] map = new int[256];
        int num = 0; // 记录有多少个distinct character
        int max = 0;
        int leftBound = 0;
        
        for(int i = 0; i < s.length(); i ++) {
            if(map[s.charAt(i)]++ == 0) num ++;
            
            while(num > k) {
                // move left bound
                if(--map[s.charAt(leftBound++)] == 0) num --;
            }
            max = Math.max(max, i - leftBound + 1);
        }
        return max;
    }
}
```



