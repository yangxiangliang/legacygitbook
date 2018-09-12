# 249. Group Shifted Strings

> Given a string, we can "shift" each of its letter to its successive letter, for example: `"abc" ->"bcd"`. We can keep "shifting" which forms the sequence:
>
> ```
> "abc" ->"bcd" ->... ->"xyz"
> ```
>
> Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.
>
> For example, given:`["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"]`,
>
> A solution is:
>
> ```
> [
>   ["abc","bcd","xyz"],
>   ["az","ba"],
>   ["acef"],
>   ["a","z"]
> ]
> ```

### 思路

这里容易看出来，同一个shifting sequence的string都可以转化为同一个string，比如都可以转化为以"a"开头的string，比如例子中的第1行，都可以shift为"abc", 第二行都可以shift为"az"。所以可以依次作为hashMap的key，然后同一个key的string，加入hashMap的value中，该value用list&lt;String&gt;记录即可，最后返回所有的hashMap的values。

### Code

```java
public class Solution {
    public List<List<String>> groupStrings(String[] strings) {
        HashMap<String, List<String>> map = new HashMap();
        List<List<String>> rst = new ArrayList();

        for(String str : strings) {
            String tmp = getShiftString(str);
            if(!map.containsKey(tmp)) map.put(tmp, new ArrayList());
            map.get(tmp).add(str);
        }
        for(List<String> list : map.values()) rst.add(list);
        return rst;
    }

    public String getShiftString(String s) {
        char[] chars = new char[s.length()];
        int offset = s.charAt(0) - 'a';
        for(int i = 0; i < s.length(); i ++) {
            // 这里需要cast成 (char)，因为s.charAt(i) 是把其当做int来看的
            char tmp = (char) (s.charAt(i) - offset);
            // 注意"az", "ba"的情况，tmp 可能比'a'小，那么需要加上26得到正确的char
            chars[i] = (tmp < 'a') ? (char) (tmp + 26) : tmp;
        }
        return new String(chars);
    }
}
```



