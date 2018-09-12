# 68. Text Justification

> Given an array of words and a width _maxWidth_, format the text such that each line has exactly \_maxWidth \_characters and is fully \(left and right\) justified.
>
> You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces   `' '`when necessary so that each line has exactly \_maxWidth \_characters.
>
> Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.
>
> For the last line of text, it should be left justified and no **extra **space is inserted between words.
>
> **Note:**
>
> * A word is defined as a character sequence consisting of non-space characters only.
> * Each word's length is guaranteed to be greater than 0 and not exceed _maxWidth_
> * The input array **words** contains at least one word.

### 思路

1. 思路很简单，就是根据题目意思“演绎” 法即可。把input的words一行一行排列即可。但是排列的时候需要注意几点。
2. 首先记录对于结果中的每一行，到底可以放入多少个words，总的长度才能不超过maxWidth。注意每2个Word中间至少会有一个space，这个也需要计入被占用的长度。
3. 然后计算每一行还需要放入多少剩余的space把该行“填满”，使长度为maxWidth。此时需要得知该行一共能放入多少words的count，然后\(count - 1\) 表示有多少words之间的gap可以用来放入多余的space。每个gap 能放入的多余的space的数量就是\(剩余的总space数量 / gap的数量\)。如果 \(剩余的总space数量 % gap的数量 &gt; 0\)，那么需要把这些多的space从左边的gap到右边的gap，每个gap放入一个space，直到插入所有space为止。因为题目要求左边的slots可以插入比右边的slots多的space。

### Code

```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> rst = new ArrayList();
        int start = 0; // 表示能插入当前行的word的start index

        while(start < words.length) {
            // 记录插入的Word和space占当前行的char个数
            int count = 0;
            // end index 表示不能插入当前行的Word，只能插入下一行
            int end = start;

            while(end < words.length) {
                // count == 0的时候，不用在该Word前插入space，因为其为该行的起始Word
                // 反之，则需要在该Word前插入1个space
                if(count == 0 && count + words[end].length() <= maxWidth) count += words[end++].length();
                else if (count + words[end].length() + 1 <= maxWidth) count += words[end++].length() + 1;
                else break;
            }

            int gaps = end - start - 1; // number of gaps between words
            StringBuilder cur = new StringBuilder();

            // last line or this line only contains one word, so left justified anyway
            if(gaps == 0 || end == words.length) {
                for(int i = start; i < end; i ++) {
                    cur.append(words[i]);
                    if(i < end - 1) cur.append(' ');
                }
                // 因为这里left justified，所以后面都padding space
                for(int i = count; i < maxWidth; i ++) {
                    cur.append(' ');
                }
            } else {
                // 表示需要在当前行增加的多余的space
                int extraSpaces = maxWidth - count;
                // 把多余的space平均分摊到每2个Word中的gap，看每个gap需要增加几个space
                int extraSpaceForEach = extraSpaces / gaps;

                for(int i = start; i < end; i ++) {
                    cur.append(words[i]);
                    if(i < end - 1) {
                        cur.append(' ');
                        for(int j = 0; j < extraSpaceForEach; j ++) {
                            cur.append(" ");
                        }
                    }
                    // 多余的space不能平均分摊到每2个Word的gap中，需要从左边的gap到右边 gap一个一个插入剩下的space
                    if(i - start < extraSpaces % gaps) cur.append(' ');
                }
            }

            rst.add(cur.toString());
            start = end;
        }
        return rst;
    }
}
```



