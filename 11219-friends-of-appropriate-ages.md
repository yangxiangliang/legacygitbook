# 825. Friends Of Appropriate Ages

> Some people will make friend requests. The list of their ages is given and `ages[i]` is the age of the ith person.
>
> Person A will NOT friend request person B \(B != A\) if any of the following conditions are true:
>
> * age\[B\] &lt;= 0.5 \* age\[A\] + 7
> * age\[B\] &gt; age\[A\]
> * age\[B\] &gt; 100 && age\[A\] &lt; 100
>
> Otherwise, A will friend request B.
>
> Note that if A requests B, B does not necessarily request A.  Also, people will not friend request themselves.
>
> How many total friend requests are made?
>
> **Example 1:**
>
> ```
> Input: [16,16]
> Output: 2
> Explanation: 2 people friend request each other.
> ```
>
> **Example 2:**
>
> ```
> Input: [16,17,18]
> Output: 2
> Explanation: Friend requests are made 17 -> 16, 18 -> 17.
> ```
>
> **Example 3:**
>
> ```
> Input: [20,30,100,110,120]
> Output: 
> Explanation: Friend requests are made 110 -> 100, 120 -> 110, 120 -> 100.
> ```
>
> Notes:
>
> * 1 &lt;= ages.length &lt;= 20000.
> * 1 &lt;= ages\[i\] &lt;= 120.

### 思路

1. 首先观察题目中的小条件，来分析什么情况下才会有friend request。发现题目中有第二个条件的情况下，第三个条件是没有必要的。因为第三个条件其实也就是"age\[B\] &gt; age\[A\]" 的情况。所以就剩下2个条件了，相当于对于年龄为 i 的人，不会给 age &gt; i 的人发friend request；也不会给 age &lt;= 0.5 \* i + 7 的人发friend request。所以就是给年纪在 \(0.5 \*i  + 7, i \] 范围里面的人发friend request。所以这里容易想到首先需要记录每个年纪的人数，那么可以用 “buckets sort”的思想，每个bucket 对应的是该年纪的人数。
2. 光有buckets 记录每个年纪的人数还是不够，不能快速的找到年纪在 \(0.5 \* i  + 7, i \] 范围里面的人数，即 年纪为 i 的人需要发送的friend request的个数。那么需要一个buckets \(int array\) 来记录 \(age &lt;= i\) 的所有人的个数。即 buckets\[i\] 表示 age &lt;= i 的人数。 那么需要求 \(a, b\] 年龄段 的人数即用 buckets\[b\] - buckets\[a\] 即可。

### Code

```java
class Solution {
    public int numFriendRequests(int[] ages) {
        int[] numInAge = new int[121]; // 因为年纪到120岁封底，所以index 取 0 - 120 即可
        int[] sumInAge = new int[121];

        for(int i : ages) numInAge[i] ++;
        sumInAge[0] = numInAge[0];
        for(int i = 1; i <= 120; i ++) {
            sumInAge[i] = numInAge[i] + sumInAge[i-1];
        }

        int rst = 0;
        for(int i = 0; i <= 120; i ++) {
            int left = i / 2 + 7;
            // left >= i 的时候，表示年纪为 i 的人不能给人发friend request
            if(left >= i || numInAge[i] == 0) continue;
            // 计算年纪为 i 的人需要一共发送出去多少friend request，括号里面 (-1) 表示不自己给自己发 friend request
            rst += (sumInAge[i] - sumInAge[left] - 1) * numInAge[i];
        }
        return rst;
    }
}
```



