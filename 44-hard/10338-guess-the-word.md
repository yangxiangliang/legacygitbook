# 843. Guess the Word

> This problem is an _**interactive problem**_ new to the LeetCode platform.
>
> We are given a word list of unique words, each word is 6 letters long, and one word in this list is chosen as **secret**.
>
> You may call`master.guess(word)` to guess a word.  The guessed word should have type`string` and must be from the original list with 6 lowercase letters.
>
> This function returns an `integer` type, representing the number of exact matches \(value and position\) of your guess to the **secret word**.  Also, if your guess is not in the given wordlist, it will return`-1`instead.
>
> For each test case, you have 10 guesses to guess the word. At the end of any number of calls, if you have made 10 or less calls to`master.guess` and at least one of these guesses was the **secret**, you pass the test case.
>
> Besides the example test case below, there will be 5 additional test cases, each with 100 words in the word list.  The letters of each word in those test cases were chosen independently at random from`'a'`to`'z'`, such that every word in the given word lists is unique.
>
> ```
> Example 1:
> Input: secret = "acckzz", wordlist = ["acckzz","ccbazz","eiowzz","abcczz"]
>
> Explanation:
>
> master.guess("aaaaaa") returns -1, because "aaaaaa" is not in wordlist.
> master.guess("acckzz") returns 6, because "acckzz" is secret and has all 6 matches.
> master.guess("ccbazz") returns 3, because "ccbazz" has 3 matches.
> master.guess("eiowzz") returns 2, because "eiowzz" has 2 matches.
> master.guess("abcczz") returns 4, because "abcczz" has 4 matches.
>
> We made 5 calls to master.guess and one of them was the secret, so we pass the test case.
> ```

### 思路

1. 每次从word list 里面随机找一个Word 出来guess，然后知道正确结果和其有 x 个character match。那么把剩下的word当中和其x 个character match的word找出来，他们是potential的结果。把这些potential的结果组成新的word list，再重复之前的操作来guess。

### Code

```java
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface Master {
 *     public int guess(String word) {}
 * }
 */
class Solution {
    public void findSecretWord(String[] wordlist, Master master) {
        for(int i = 0, x = 0; i < 10 & x < 6; i ++) {
            String tmp = wordlist[new Random().nextInt(wordlist.length)];
            x = master.guess(tmp);
            List<String> newList = new ArrayList();
            for(String word : wordlist) {
                if(match(tmp, word) == x) newList.add(word);
            }
            wordlist = newList.toArray(new String[newList.size()]);
        }
    }

    private int match(String a, String b) {
        int count = 0;
        for(int i = 0; i < a.length(); i ++) {
            if(a.charAt(i) == b.charAt(i)) count ++;
        }
        return count;
    }
}
```



