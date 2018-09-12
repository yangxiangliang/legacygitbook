# 465. Optimal Account Balancing

> A group of friends went on holiday and sometimes lent each other money. For example, Alice paid for Bill's lunch for $10. Then later Chris gave Alice $5 for a taxi ride. We can model each transaction as a tuple \(x, y, z\) which means person x gave person y $z. Assuming Alice, Bill, and Chris are person 0, 1, and 2 respectively \(0, 1, 2 are the person's ID\), the transactions can be represented as`[[0, 1, 10], [2, 0, 5]]`.
>
> Given a list of transactions between a group of people, return the minimum number of transactions required to settle the debt.
>
> **Note: **
>
> 1. A transaction will be given as a tuple \(x, y, z\). Note that x ? y and z &gt; 0.
>
> 2. Person's IDs may not be linear, e.g. we could have the persons 0, 1, 2 or we could also have the persons 0, 2, 6.
>
> **Example 1: **
>
> ```
> Input:
> [[0,1,10], [2,0,5]]
>
> Output:
> 2
>
> Explanation:
> Person #0 gave person #1 $10.
> Person #2 gave person #0 $5.
>
> Two transactions are needed. One way to settle the debt is person #1 pays person #0 and #2 $5 each.
> ```
>
> **Example 2:**
>
> ```
> Input:
> [[0,1,10], [1,0,1], [1,2,5], [2,0,5]]
>
> Output:
> 1
>
> Explanation:
> Person #0 gave person #1 $10.
> Person #1 gave person #0 $1.
> Person #1 gave person #2 $5.
> Person #2 gave person #0 $5.
>
> Therefore, person #1 only need to give person #0 $4, and all debt is settled.
> ```

### 思路

这个题目较难，首先根据input的 int\[\]\[\] transactions，可以给每个人都create一个balance，balance为positive的话，则说明该人应该收回钱；如果balance为negative的话，则说明该人应该还别人钱，这里用一个hashMap记录即可。然后找出其中balance不为0的人即可，然后把balance都放入一个数组中。注意这里因为题目里面说人的编号不是linear，所以放入balance的数组的index不用和人的编号匹配，只要知道每个index对应不同的人即可，人的编号在此题中并没有什么用。然后其实就是用recursion遍历所有的情况，从account\[0\]开始，如果其balance的符号和account\[i\] 相反，那么让其transaction到account\[i\]，则account\[i\] 值变为 account\[i\] + account\[0\]，此时account\[0\]的账号相当于清零了。用同样的方法往后找，1,2,.....i,....n 需要多少次transactions，再加上account\[0\]的这次transaction即可。注意account\[0\]可能和1，2，......i, ......n 任何一个符号相反的balance组合，所以考虑所有情况中，然后取transaction次数最少的即可。

### Code

```java
public class Solution {
    public int minTransfers(int[][] transactions) {
        HashMap<Integer, Integer> map = new HashMap();
        for(int[] trans : transactions) {
            map.put(trans[0], map.getOrDefault(trans[0], 0) + trans[2]);
            map.put(trans[1], map.getOrDefault(trans[1], 0) - trans[2]);
        }
        
        List<Integer> list = new ArrayList();
        for(int key : map.keySet()) {
            if(map.get(key) != 0) list.add(map.get(key));
        }
        
        int[] accounts = new int[list.size()];
        for(int i = 0; i < list.size(); i ++) accounts[i] = list.get(i);
        return helper(accounts, 0, 0);
    }
    
    public int helper(int[] accounts, int start, int count) { // count该recursion function之前已经transactions的次数
        int rst = Integer.MAX_VALUE;
        // skip掉可能已经被 "清零"的balance
        while(start < accounts.length && accounts[start] == 0) start ++;
        
        for(int i = start + 1; i < accounts.length; i ++) {
            // 注意account[i] , account[start]需要符号相反才能"抵消"
            if(accounts[i] * accounts[start] < 0) {
                accounts[i] += accounts[start];
                // 注意这里需要(start+1)，因为start已经被清零，然后从下一个开始
                // (count+1) 因为多了1次 accounts[start]的transaction
                rst = Math.min(rst, helper(accounts, start + 1, count + 1));
                accounts[i] -= accounts[start]; // 注意recursion完后，需要把account[i]恢复到原来，不然之后的loop用其改变后的值
            }
        }
        return rst == Integer.MAX_VALUE ? count : rst;
    }
}
```



