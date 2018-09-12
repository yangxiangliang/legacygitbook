# 301. Remove Invalid Parentheses

> Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.
>
> Note: The input string may contain letters other than the parentheses`(`and`)`.
>
> **Example:**
>
> ```
> "()())()" -> ["()()()", "(())()"]
> "(a)())()" -> ["(a)()()", "(a())()"]
> ")(" -> [""]
> ```

### 思路 1）DFS

1. 首先熟悉判断一个string是不是有效括号的方法：就是用一个counter数，遇见左括号“（” 的话，counter+1；遇见右括号“）”的话，counter - 1。任一位置的时候，counter肯定都不可能为negative，而且最后遍历完整个string后的counter肯定要等于0。因为这里要求remove掉minimum的情况，所以需要一开始找出有多少个“多于”的左括号和右括号。remove minimum的情况，就是把这些“多余”的括号都remove掉就行。
   1. 这里用2个var记录多余的left 的数量和right的数量。**注意不能用1个counter来记录是该remove 左括号，还是右括号。比如情况"\)\("，其实左括号和右括号的数量是一样的，用1个counter的话，其就等于0。感觉不用remove，其实需要remove，这里多余的右括号是1个，多余的左括号也是1个，所以要把2个都remove掉，得到是empty string ""。**
   2. 计算“多余”的left 括号和 right 括号的方法：如果当前char是‘（’，那么left ++；如果当前char ‘）’，那么要看left的值，如果left == 0，表示left 和 right已经匹配，那么right ++，表示找到了一个多余的right括号。如果left != 0，那么left - -，表示找到了一个与left匹配的right括号，多余的左括号数量-1。
2. 当得知”多余“的左括号和右括号的数量时，那么就遍历s，一个一个尝试remove掉当前的char，看是否得到valid的结果。这里便是DFS的感觉。

3. 注意在DFS的时候，为了不每次都从string s的开始遍历，recursion的时候可以parse index，表示当前遍历到的位置，这样就不用每次从头开始遍历了。

4. 还要避免重复的结果，最容易的方法就是先用set记录结果，最后return的时候把set转化成list即可。但是这样速度较慢，还是有不少重复计算。

### Code

1\)

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        HashSet<String> rst = new HashSet();
        int left = 0; // 多余的left '('
        int right = 0; // 多余的right ')'
        for(char c : s.toCharArray()) {
            if(c == '(') left ++;
            if(c == ')') {
                if(left == 0) right ++;
                else left --;
            }
        }
        remove(s, rst, left, right, 0);
        return new ArrayList(rst);
    }

    private void remove(String s, HashSet<String> rst, int left, int right, int start) {
        if(left == 0 && right == 0) {
            // 注意：这里不能直接rst.add(s)，比如 “()())()”多了一个）,那么可能最后remove 
            // 掉最后的一个‘）’，得到“()())(”，虽然left == 0, right == 0,依然不是valid，所以还是要多做一次check
            if(isValid(s)) rst.add(s);
            return;
        }
        for(int i = start; i < s.length(); i ++) {
            char cur = s.charAt(i);
            // 注意这里传入下一个recursion的string不包含s.charAt(i)，所以下层开始遍历的char的index还是i
            // 相当于这一层函数里string s在index (i+1)位置的char
            if(left > 0 && cur == '(') remove(s.substring(0, i) + s.substring(i+1), rst, left - 1, right, i);
            if(right > 0 && cur == ')') remove(s.substring(0, i) + s.substring(i+1), rst, left, right -1, i);
        }
    }

    private boolean isValid(String s) {
        int count = 0;
        for(char c : s.toCharArray()) {
            if(c == '(') count ++;
            if(c == ')') count --;
            if(count < 0) return false;
        }
        return count == 0;
    }
}
```

2）

不用set来记录结果的方法来去重，因为这样依然会导致很多重复运算，想有没有其他方法在计算过程中避免掉重复运算。

首先想想什么情况下才会出现重复计算，比如"\(\)\)"去掉第2个char和第3个char得到的东西是一样的。所以如果2个连续的一样的char排在一起，那么就去点第一个char，skip掉后面连续的一样的char即可，来避免重复计算。

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> rst = new ArrayList();
        int left = 0; // 多余的left '('
        int right = 0; // 多余的right ')'
        for(char c : s.toCharArray()) {
            if(c == '(') left ++;
            if(c == ')') {
                if(left == 0) right ++;
                else left --;
            }
        }
        remove(s, rst, left, right, 0);
        return rst;
    }

    private void remove(String s, List<String> rst, int left, int right, int start) {
        if(left == 0 && right == 0) {
            if(isValid(s)) rst.add(s);
            return;
        }
        for(int i = start; i < s.length(); i ++) {
            // 多个连续的char在一起，只对第一个char处理，skip后后面连续的一样 的char
            if(i > start && s.charAt(i) == s.charAt(i-1)) continue;
            char cur = s.charAt(i);
            if(left > 0 && cur == '(') remove(s.substring(0, i) + s.substring(i+1), rst, left - 1, right, i);
            if(right > 0 && cur == ')') remove(s.substring(0, i) + s.substring(i+1), rst, left, right -1, i);
        }
    }

    private boolean isValid(String s) {
        int count = 0;
        for(char c : s.toCharArray()) {
            if(c == '(') count ++;
            if(c == ')') count --;
            if(count < 0) return false;
        }
        return count == 0;
    }
}
```

### 思路 2）BFS

1. 这里需要remove掉minimum个char，即得到的结果长度和原来string 的长度差距最小。感觉像是一个string到另外一个string找“最短路径”的感觉。找“最短路径”的题目经常想到BFS的方法，那么思考这道题目能不能用BFS来处理。
2. 如果用BFS，那么用queue来store中间状态 的string，最开始就是把input string去掉一个char，然后全部按顺序装入queue，看有没有valid的，如果有valid，那说明remove掉minimum个char就是remove掉1个，所以当前队列里面都是原来string remove掉一个char的情况，把它们check完即可，不用再考虑remove掉2个char，3个char的情况了。如果当前queue中没有1个是valid，说明光remove掉1个char是不够的，还要继续把remove掉2个char的所有substring放入queue中，直到找到valid的string。

### Code

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> rst = new ArrayList();
        HashSet<String> visited = new HashSet(); // 用set去掉结果中的duplicates
        // 表示是否在BFS的当前层中找到valid的情况，如果找到的话，就把当前BFS中queue中的string check完即可
        // 如果没有找到，还需要把remove掉更多char的substring放入queue中继续查找
        boolean found = false;

        Queue<String> queue = new LinkedList();
        queue.add(s);
        while(queue.isEmpty() == false) {
            String cur = queue.poll();
            if(visited.contains(cur)) continue;
            else visited.add(cur);

            if(isValid(cur)) {
                rst.add(cur);
                found = true;
            }
            // 看是否在BFS当前一层就发现valid string，如果有，就把当前一层BFS所发现的string check完即可
            // 不用往下process，去掉更多char了。因为题目只需要remove掉minimum个char即可
            if(found) continue;

            for(int i = 0; i < cur.length(); i ++) {
                if(cur.charAt(i) == '(' || cur.charAt(i) == ')')
                    queue.add(cur.substring(0, i) + cur.substring(i+1));
            }
        }
        return rst;
    }

    private boolean isValid(String s) {
        int count = 0;
        for(char c : s.toCharArray()) {
            if(c == '(') count ++;
            if(c == ')') count --;
            if(count < 0) return false;
        }
        return count == 0;
    }
}
```



