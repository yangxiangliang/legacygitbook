# 345. Reverse Vowels of a String

> Write a function that takes a string as input and reverse only the vowels of a string.
>
> **Example 1:**
>
> Given s = "hello", return "holle".
>
> **Example 2: **
>
> Given s = "leetcode", return "leotcede".
>
> **Note:**
>
> The vowels does not include the letter "y".

### 思路

典型的2个pointer，一个从s的开始，一个从s的末尾向中间移动，如果遇见vowel，则交换character位置。**这里一开始将s转化成char array比较方便，方便swap不同位置的char.**

### Code

```java
public class Solution {
    public String reverseVowels(String s) {
        HashSet<Character> vowels = new HashSet();
        vowels.add('a');
        vowels.add('e');
        vowels.add('i');
        vowels.add('o');
        vowels.add('u');
        
        int start = 0;
        int end = s.length() - 1;
        char[] chars = s.toCharArray();
        while(start < end) {
            if(vowels.contains(Character.toLowerCase(chars[start]))) {
                while(start < end && !vowels.contains(Character.toLowerCase(chars[end]))) end --;
                
                if(start < end && chars[start] != chars[end]) {
                    char temp = chars[start];
                    chars[start] = chars[end];
                    chars[end] = temp;
                }
                // 该end位置已经交换过了，所以end --
                end --;
            }
            // 这里不用end --，因为end可能是vowel，但是start不是，所以不移动end pointer
            start ++;
        }
        return new String(chars);
    }
}
```



