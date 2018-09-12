# 392. Is Subsequence

> Given a string **s **and a string **t**, check if **s **is subsequence of **t**.
>
> You may assume that there is only lower case English letters in both **s **and **t**. **t **is potentially a very long \(length ~= 500,000\) string, and **s **is a short string \(&lt;=100\).
>
> A subsequence of a string is a new string which is formed from the original string by deleting some \(can be none\) of the characters without disturbing the relative positions of the remaining characters. \(ie,`"ace"`is a subsequence of`"abcde"`while`"aec"`is not\).
>
> **Example 1:**  
> **s**=`"abc"`, **t**=`"ahbgdc"`
>
> Return`true`.
>
> **Example 2:**  
> **s**=`"axc"`, **t**=`"ahbgdc"`
>
> Return`false`.
>
> **Follow up:**  
> If there are lots of incoming S, say S1, S2, ... , Sk where k &gt;= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?

### 思路

一开始想到最简单的写法就是recursion，先在 t 中找与 s 中第一个字母相同的字母，假设此char在 t 中的index为i。那么如果s 是 t的subsequence的话，s 除去第一个字母的substring 也必须是 t 在index i 之后的substring的subsequence。所以就找到可以recursion来处理。

### Code

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if(s.length() == 0) return true;
        if(s.length() > t.length()) return false;
        for(int i = 0; i < t.length(); i ++) {
            if(s.charAt(0) == t.charAt(i) && isSubsequence(s.substring(1), t.substring(i+1))) return true;
        }
        return false;
    }
}
```

### Follow up

上面的做法，没法解决 follow up。因为每个recursion 都有for loop要scan t string，而且题目说了 t string会非常长。显然不能每次s string 出现的时候，都去scan t string。那么显然需要预处理一下 t string，然后每次 s string出现的时候，用预处理的结果能不能快速找出其是否是subsequence 即可。

1. 先回归问题，其实就是找 s 中的character是否能在 t 中都存在，则存在的index是依次排列的问题。这里找寻 “一个东西在一个array里面是否存在，并且找到其存在的index” 类问题，efficient的方法就容易往 binary search上面思考。
2. 显然不要去想对整个 t 做binary search，因为题目中明确说了 t 是很长的，对其直接binary search 复杂度也高。那么想到预处理 t 的时候，可不可以得到t中每个character 以及该character 出现在 t 中的index。然后处理 s 的时候，先看 s.charAt\(0\) 在 t 中最靠前的index，比如是index0。然后看s.charAt\(i\) 在 t 中 index0 之后是否存在 index，依次类推即可。然后每一步找index的时候用binary search来提速。

### Code

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        HashMap<Character, List<Integer>> map = new HashMap<>();
        for(int i = 0; i < t.length(); i ++) {
            char c = t.charAt(i);
            if(!map.containsKey(c)) map.put(c, new ArrayList<>());
            map.get(c).add(i);
        }

        int pre = -1;
        for(int i = 0; i < s.length(); i ++) {
            char c = s.charAt(i);
            pre = binarySearch(map.getOrDefault(c, new ArrayList<>()), pre);
            if(pre == -1) return false;
        }
        return true;
    }

    // 找list中是否有比target 大的integer index，没有的话则返回 -1
    private int binarySearch(List<Integer> list, int target) {
        if(list.size() == 0) return -1;
        int start = 0, end = list.size() - 1;
        while(start < end) {
            int mid = (start + end) / 2;
            if(target == list.get(mid)) return mid == list.size() - 1 ? -1 : list.get(mid + 1);
            if(target > list.get(mid)) start = mid + 1;
            // target < list.get(mid) 的时候，mid 仍然有可能是最后合理结果，所以这里是end = mid,而不是 end = mid - 1
            else end = mid;
        }
        // 找出来的结果必须大于target value，比如 list.size() == 1 的时候，可能结果不大于target，则返回 -1
        return target > list.get(start) ? -1 : list.get(start);
    }
}
```



