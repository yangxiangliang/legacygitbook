# 418. Sentence Screen Fitting

> Given a `rows x cols`screen and a sentence represented by a list of **non-empty **words, find **how many times **the given sentence can be fitted on the screen.
>
> **Note:**
>
> 1. A word cannot be split into two lines.
> 2. The order of words in the sentence must remain unchanged.
> 3. Two consecutive words **in a line **must be separated by a single space.
>
> 4. Total words in the sentence won't exceed 100.
>
> 5. Length of each word is greater than 0 and won't exceed 10.
>
> 6. 1 ≤ rows, cols ≤ 20,000.

### 思路

**1\)**  一开始想的笨办法，brute-force解决问题，下面有code。

**2）**比如 sentence = \["abc", "de", "f"\], rows = 4 and cols = 6。因为两个word之间必须隔一个space，所以如果要把这个sentence填入screen，那么相当于要填入s = "abc-de-f-"，其中的"-"表示space。注意“f” 后也有space，因为其和新的sentence的第一个字母也需要隔开。那么其实问题变成了我们可以放入多少个“abc-de-f-”这样的string。这里需要一个count 来记录已经放入screen的character的个数，那么最后的结果就是 count / s.length\(\) 。一开始，假设填满cols，那么 count += cols，表示已经填满了6个col，如果s.charAt\(count % s.length\(\)\) == ' ', 说明下一个是space，但是这里也是row的尾端，所以不用extra space也可以，并且count ++，说明已经放下了 cols + 1 个character。如果 s.charAt\(count % s.length\(\)\) != ' '，那么说明该word不能放入该row，需要找到上一个'-' space处，并且update count的值。

### Code 1\)

```java
public class Solution {
    public int wordsTyping(String[] sentence, int rows, int cols) {
        int rst = 0;
        int rowRemain = rows; // 表示还剩下多少row没有 “填上” sentence中的word
        int colRemain = cols; // 表示该row还剩下几个space需要 “填上” sentence中的word
        int index = 0;   // 表示现在 "填"的sentence中的word的index
        while(rowRemain > 0) {
            String word = sentence[index];
            if(word.length() > colRemain) {
                colRemain = cols;
                rowRemain --;
            } else {
                colRemain = colRemain - word.length() - 1;
                if(colRemain <= 0) {
                    colRemain = cols;
                    rowRemain --;
                }
                index ++;
            }
            if(index == sentence.length) {
                rst ++;
                index = 0;
            }
        }
        return rst;
    }
}
```

### Code 2\)

```java
public class Solution {
    public int wordsTyping(String[] sentence, int rows, int cols) {
        String s = String.join(" ", sentence) + " ";
        int n = s.length();
        int count = 0;
        for(int i = 0; i < rows; i ++) {
            count += cols;
            if(s.charAt(count % n) == ' ') count ++;
            else {
                while(count > 0 && s.charAt((count - 1) % n) != ' ') count --;
            }
        }
        return count / n;
    }
}
```



