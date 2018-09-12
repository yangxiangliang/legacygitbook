# 411. Minimum Unique Word Abbreviation

> A string such as`"word"`contains the following abbreviations:
>
> ```
> ["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
> ```
>
> Given a target string and a set of strings in a dictionary, find an abbreviation of this target string with the **smallest possible **length such that it does not conflict with abbreviations of the strings in the dictionary.
>
> Each **number **or letter in the abbreviation is considered length = 1. For example, the abbreviation "a32bc" has length = 4.
>
> **Note:**
>
> * In the case of multiple answers as shown in the second example below, you may return any one of them.
> * Assume length of target string = **m**, and dictionary size = **n. **You may assume that **m&lt;=21, n&lt;=1000**, and **log\(n\)+m&lt;=20**
>
> **Example:**
>
> ```
> "apple", ["blade"] -> "a4" (because "5" or "4e" conflicts with "blade")
>
> "apple", ["plain", "amber", "blade"] ->"1p3" (other valid answers include "ap3", "a3e", "2p2", "3le", "3l1").
> ```

### 思路

这道题目可以借用 [10.2.18 Generalized Abbreviation](/43-medium/10218-generalized-abbreviation.md) 中的思路来构建target的abbreviation，然后对其所有的abbreviation string检查，其和dictionary中的word有没有duplicate的abbreviation string。

### 注意

* 如果target string和dictionary 中word的长度不同，则它们是不可能有相同的abbreviation的，所以可以直接skip掉dictionary中和target长度不同的string
* 为了更快，应该是从target string的所有abbreviation中从长度最短到最长的顺序来和dictionary中的string比较，如果有符号要求的abbreviation，直接return 为结果即可，不用遍历更长的abbreviation了。
  * 可以将target的所有abbreviation 按照长度sort，从短到长开始遍历。
  * 也可以在build target的abbreviation的过程中，将其放入1个heap，该heap是按照string长度的minHeap，之后直接从这个heap中1个1个取abbreviation即可。
  * 下面的处理的方法只是用1个global variable表示结果 string: rst。在构建target的abbreviation过程中，如果正在构建的string的长度已经大于rst的长度，那么停止构建，因为就算其符合要求，也是更长的string。

### Code

```java
public class Solution {
    String rst = "";
    
    public String minAbbreviation(String target, String[] dictionary) {
        buildAbbre(target, dictionary, "", 0, 0);
        return rst;
    }
    
    // build target的abbreviation，类似 10.2.18中DFS方法
    public void buildAbbre(String target, String[] dictionary, String cur, int position, int count) {
        // 如果 cur string已经长于 rst，那么停止后面的构建
        if(rst.length() > 0 && cur.length() >= rst.length()) return;
        
        if(position == target.length()) {
            if(count > 0) cur += count;
            for(String word : dictionary) {
                // 只用 check长度和target一样的word即可
                if(word.length() == target.length() && hasAbbre(cur, word)) return;
            }
            if(rst.length() == 0 || cur.length() < rst.length()) rst = cur;
            return;
        }
        
        buildAbbre(target, dictionary, cur, position+1, count+1);
        buildAbbre(target, dictionary, cur+(count > 0 ? count : "") + target.charAt(position), position+1, 0);
    }
    
    public boolean hasAbbre(String abbr, String word) {
        int i = 0; // index in abbr String
        int j = 0;  // index in word String
        while(i < abbr.length()) {
            int count = 0;
            while(i < abbr.length() && Character.isDigit(abbr.charAt(i))) {
                count = 10 * count + Character.getNumericValue(abbr.charAt(i));
                i ++;
            }
            if(i == abbr.length()) break;
            
            j += count;
            if(abbr.charAt(i) != word.charAt(j)) return false;
            i++;
            j++;
        }
        return true;
    }
}
```



