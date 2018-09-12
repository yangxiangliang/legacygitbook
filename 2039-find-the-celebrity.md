# 277. Find the Celebrity

> Suppose you are at a party with`n`people \(labeled from`0`to`n - 1`\) and among them, there may exist one celebrity. The definition of a celebrity is that all the other`n - 1`people know him/her but he/she does not know any of them.
>
> Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity \(or verify there is not one\) by asking as few questions as possible \(in the asymptotic sense\).
>
> You are given a helper function`bool knows(a, b)`which tells you whether A knows B. Implement a function`int findCelebrity(n)`, your function should minimize the number of calls to`knows`.
>
> **Note: **There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return`-1`.

### 思路

1. 最容易想到的就是brute force的方法，遍历所有2个人的组合情况，然后用2个hashMap记录每个人认识的其他人的个数，和有多少其他人认识自己的人数。最后遍历hashMap找出celebrity即可。但是这个复杂度是O\(N^2\)，想一想有没有其他办法不需要对于每2个人都用knows\(\)函数比较的。
2. 注意题目中celebrity的定义，如果“a认识b，那么说明a不会是celebrity”，因为celebrity不认识任何人。如果“a不认识b，说明b不会是celebrity”，因为其他所有人应该都认识celebrity。所以相当于对于任何 a, b 两人用 knows\(a, b\) 之后，可以排除其中1个人不是celebrity，一个可能是celebrity。所以是否有办法先找出可能是celebrity的candidate，然后再判断该candidate是否是celebrity。

### Code 1\)

先用stack 记录所有人，然后pop出其中2人，对该2人用knows\(\)函数。可以排除其中一个不是celebrity，把另外一个可能是celebrity的candidate重新压入stack中。最后得到1个可能是celebrity的candidate。

### 复杂度

Time: O\(n\);   Space: O\(n\)

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
    public int findCelebrity(int n) {
        Stack<Integer> stack = new Stack();
        for(int i = 0; i < n; i ++) stack.push(i);
        while(stack.size() > 1) {
            int a = stack.pop();
            int b = stack.pop();
            if(knows(a, b)) stack.push(b);
            else stack.push(a);
        }

        int rst = stack.pop();
        for(int i = 0; i < n; i ++) {
            if(i != rst) {
                if(knows(rst, i) || !knows(i, rst)) return -1;
            }
        }
        return rst;
    }
}
```

### Code 2\)

还可以简化找candidate的过程。不用stack的额外空间。就初始化candidate 为 0，然后对\(1, n-1\) 做one-pass 遍历，如果knows\(candidate, i\) == true，说明candidate错误，i 可能是celebrity，所以switch candidate为i。如果 knows\(candidate, i\) == false, 说明i 不可能是celebrity，所以keep 当前candidate的值。one-pass 后，便可以找到 potential的candidate的值。

### 复杂度

Time: O\(n\); Space: O\(1\)

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
    public int findCelebrity(int n) {
        int candidate = 0;
        // find out candidate firstly, if (knows(candidate, i)), candidate should be wrong because celebrity knows nobody
        // change candidate to i, if (knows(candidate, i) == false), i should not be celebrity, keep candidate
        for (int i=1; i<n; i++) {
            if (knows(candidate, i)) {
                candidate = i;
            }
        }
        for (int i=0; i<n; i++) {
            if (i != candidate) {
                if (!knows(i, candidate) || knows(candidate, i)) return -1;
            }
        }
        return candidate;
    }
}
```



