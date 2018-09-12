# 320. Generalized Abbreviation

> Write a function to generate the generalized abbreviations of a word.
>
> **Example: **
>
> Given word = `"word"`, return the following list \(order does not matter\):
>
> ```
> ["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
> ```

### 思路  1\)

典型的backtrack，需要用到recursion。观察题目，发现要得到word的abbreviation list，可以对"w" abbreviation，也可以不abbreviation。即相当于 “w” + "ord"的abbreviation list的组合，以及 “1” + “o” + "rd"的abbreviation list的组合。因为对“w" abbreviation 后，便不能对"o" 作abbreviation，不能出现"11"，只能为"2"。所以最后的结果是 abbreviation "word"的前i 个character，然后加上从index \(i + 1\)开始的substring的abbreviation list的组合。

### Code  1\)

```java
public class Solution {
    // 避免generate 重复的string的abbreviation list
    HashMap<String, List<String>> map = new HashMap();

    public List<String> generateAbbreviations(String word) {
        if(map.containsKey(word)) return map.get(word);

        List<String> rst = new ArrayList();
        if(word.length() == 0) {
            rst.add("");
            return rst;
        }

        for(int i = 0; i < word.length(); i ++) {
            for(String tmp : generateAbbreviations(word.substring(i+1))) {
                // 说明 首字母不能abbreviation
                if(i == 0) rst.add(word.charAt(0) + tmp);
                // 对前i个character作 abbreviation
                else rst.add(String.valueOf(i) + word.charAt(i)+ tmp);
            }
        }

        // 需要加上 word.length() string，因为上面for循环中没有包括该情况
        rst.add(String.valueOf(word.length()));
        map.put(word, rst);
        return rst;
    }
}
```

### 思路  2\)

用一个helper function 来做backtracking，因为遇见一个character的时候有两种选择，要么keep it as character，要么abbreviation，除此之外，还需要一个count来记录到目前position的character时候，用多少相连的character被abbreviation了。

### 复杂度

Time: O\(2^n\)，n为word的长度，因为每个character有2种可能，所以一共有2^n 种组合需要create。

### Code 2\)

```java
public class Solution {
    public List<String> generateAbbreviations(String word) {
        List<String> rst = new ArrayList();
        buildHelper(rst, word, 0, "", 0);
        return rst;
    }
    
    public void buildHelper (List<String> rst, String word, int position, String cur, int count) {
        if(position == word.length()) {
            if(count > 0) cur += count;
            rst.add(cur);
        } else {
            buildHelper(rst, word, position + 1, cur, count + 1); // abbreviation current character
            buildHelper(rst, word, position + 1, cur + (count > 0 ? count : "") + word.charAt(position), 0); // keep current char
        }
    } 
}
```



