# 394. Decode String

> Given an encoded string, return it's decoded string.
>
> The encoding rule is:`k[encoded_string]`, where theencoded\_stringinside the square brackets is being repeated exactlyktimes. Note thatkis guaranteed to be a positive integer.
>
> You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.
>
> Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers,k. For example, there won't be input like`3a`or`2[4]`.
>
> **Example: **
>
> s = "3\[a\]2\[bc\]", return "aaabcbc".
>
> s = "3\[a2\[c\]\]", return "accaccacc".
>
> s = "2\[abc\]3\[cd\]ef", return "abcabccdcdcdef".

### 思路

有点像类似于 “括号”的问题，想到可以用stack解决。code过程中发现把number单独放入一个stack会比较好解决问题。所以考虑用2个stack，一个放number，一个放string。如果遇见了"\]"，那么就pop出之前的string直到遇见“\[”，然后pop出number看需要repeat该string多少次。repeat该string结束后，再将其push入stack，如此往复。最后pop出stack中所有的string，按顺序构造result string即可。

### Code

```java
public class Solution {
    public String decodeString(String s) {
        Stack<String> stack = new Stack();
        Stack<Integer> count = new Stack();
        int index = 0;

        while(index < s.length()) {
            // 构造repeat times的number，将其push入count stack
            // 注意不仅有“3”，还可能有“100”，所以可能连着好几位都是digit
            if (Character.isDigit(s.charAt(index))) {
                int num = 0;
                while(Character.isDigit(s.charAt(index))) {
                    num = 10 * num + s.charAt(index) - '0';
                    index ++;
                }
                count.push(num);
            }

            if(s.charAt(index) == ']') {
                StringBuilder stringToRepeat = new StringBuilder();
                // 注意pop出的string构造的顺序，靠后pop出的string应该加在结果string的前部
                while(!stack.peek().equals("[")) stringToRepeat.insert(0, stack.pop());

                stack.pop(); // pop out "["
                StringBuilder repeatString = new StringBuilder();
                int dup = count.pop();
                for(int i = 0; i < dup; i ++) repeatString.append(stringToRepeat.toString());
                stack.push(repeatString.toString());
            } else stack.push(s.charAt(index) + "");
            index ++;
        }

        StringBuilder rst = new StringBuilder();
        while(!stack.isEmpty()) rst.insert(0, stack.pop());
        return rst.toString();
    }
}
```



