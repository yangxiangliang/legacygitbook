# 336. Palindrome Pairs

> Given a list of **unique **words, find all pairs of **distinct **indices`(i, j)`in the given list, so that the concatenation of the two words, i.e.`words[i] + words[j]`is a palindrome.

### 思路

最容易想到的是brute force的方法，就是每两个word都concatenate起来，然后判断其是不是palindrome。但是这样太慢。仔细观察看能不能不用2个嵌套的for loop都遍历所有word，首先用一个hashMap记录word以及其index，方便loop up。然后对于某个word，遍历其subString，例如**"asll"，**其subString **"ll"** 也是palindrome，如果需要另外一个word和其concatenate起来是palindrome，那么该word应该是**“asll”**中除了**"ll"**之外的另外一部分subString **"as"**的reverse string，即**"sa"**。则**"asll"**和**"sa"**的index便可加入最后结果。**特别情况，**对于“bat”，可以看成由subString "bat"和empty string 组成，认为empty string也是palindrome，只需要找所有word中是否存在"bat"的reverse string即可。

**Time Complexity:** O\(n \* k^2\), n 是words的个数，k是words中每个word的平均长度

### Code

```java
public class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        List<List<Integer>> rst = new ArrayList();
        HashMap<String, Integer> map = new HashMap();
        for (int i = 0; i < words.length; i ++) map.put(words[i], i);
        for (int i = 0; i < words.length; i ++) {
            // 注意 j 取值从 0 到 words[i].length()的长度都可以
            for (int j = 0; j <= words[i].length(); j ++) {
                String first = words[i].substring(0, j);
                String second = words[i].substring(j, words[i].length());

                if (isPalindrome(first)) {
                    // StringBuilder() 有 reverse function来构造reversed string
                    String secondRev = new StringBuilder(second).reverse().toString();
                    if (map.containsKey(secondRev) && map.get(secondRev) != i) {
                        List<Integer> tmp = new ArrayList();
                        tmp.add(map.get(secondRev));
                        tmp.add(i);
                        rst.add(tmp);
                    }
                }

                if (isPalindrome(second)) {
                    String firstRev = new StringBuilder(first).reverse().toString();
                    // 由于j的取值范围，first，second string都可能为empty string，而empty string是palindrome
                    // 在first string 为empty string时，上面已经考虑这种情况了，这里不考虑second string为empty的情况来avoid duplicate
                    if (map.containsKey(firstRev) && map.get(firstRev) != i && second.length() > 0) {
                        List<Integer> tmp = new ArrayList();
                        tmp.add(i);
                        tmp.add(map.get(firstRev));
                        rst.add(tmp);
                    }
                }
            }
        }
        return rst;
    }

    public boolean isPalindrome(String word) {
        for (int i = 0; i < word.length()/2; i ++) {
            if(word.charAt(i) != word.charAt(word.length() - 1 - i)) return false;
        }
        return true;
    }
}
```



