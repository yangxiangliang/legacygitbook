# 309. Best Time to Buy and Sell Stock with Cooldown

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> Design an algorithm to find the maximum profit. You may complete as many transactions as you like \(ie, buy one and sell one share of the stock multiple times\) with the following restrictions:
>
> * You may not engage in multiple transactions at the same time \(ie, you must sell the stock before you buy again\).
> * After you sell your stock, you cannot buy stock on next day. \(ie, cooldown 1 day\)
>
> **Example:**
>
> ```
> prices = [1, 2, 3, 0, 2]
> maxProfit = 3
> transactions = [buy, sell, cooldown, buy, sell]
> ```

### 思路  1\)

一开始看见这个题目想到用 **DP**，但是没有想到具体 **DP** 怎么用。只能用了最笨的DP，但是也相当于brute force了，最后太慢过不了OJ。基本思路就是：用int\[\]\[\] dp = new int\[n\]\[n\] 记录DP过程中的值，dp\[i\]\[j\] 表示从int\[\] prices 中从 index = i 到 index = j 之间的数能产生的max profit。计算dp\[i\]\[j\] 的时候，需要遍历 i &lt; k &lt; j 的情况，k表示在index = k时候是cooldown的情况，即不buy也不sell的情况。

### Code 1\)

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if(n < 2) return 0;

        int[][] dp = new int[n][n];
        for(int i = n-1; i >= 0; i--) {
            for(int j = i; j < n; j++) {
                if (j == i) dp[i][j] = 0;
                else {
                    dp[i][j] = prices[j] - prices[i];
                    for(int k = i; k <= j; k++) {
                        if(k == i) dp[i][j] = Math.max(dp[i][j], dp[k+1][j]);
                        else if(k == j) dp[i][j] = Math.max(dp[i][j], dp[i][k-1]);
                        else dp[i][j] = Math.max(dp[i][j], dp[i][k-1] + dp[k+1][j]);
                    }
                }
            }
        }        
        return dp[0][n-1];
    }
}
```

### 思路  2\)

显然上面的DP的complexity太高了。想到能不能用1D的array来记录DP的值，比较简单。比如int\[n\] sell 来记录，sell\[i\]表示到index = i 之前最后一次操作为sell的能达到的max profit的值。但是如果只用这一个array，想来想去感觉不能解决问题，因为除了sell操作，还有buy操作，而且还可能有cooldown 操作，即rest，不buy也不sell。那么 ** 索性用3个 1D array，int\[n\] sell, int\[n\] buy, int\[n\] rest，分别表示到某个 index 之前最后一次操作为 sell, buy, rest的max profit的值 。**这样之后，推导到某个 index的时候的max profit的时候就会容易计算：

* sell\[i\] 表示到index = i 为止，最后一次操作为sell的max profit。这里分2种情况，如果sell prices\[i\]，那么需要找 \(i-1\) 之前最后一次操作为buy的max profit 然后 加上 prices\[i\] 即可；如果不sell prices\[i\]，那么其值等于 sell\[i-1\] 就行。所以 sell\[i\] = Math.max\(buy\[i-1\] + prices\[i\], sell\[i-1\]\)
* 计算buy\[i\]的值，同理也分2种情况：1\) buy prices\[i\]，因为buy之前是rest的，不能sell完了马上buy，所以就是\(i-1\)位置最后一次操作为rest的max profit 然后减去 prices\[i\] 即可；2\) 不 buy prices\[i\]，那么其max profit 就等于 buy\[i-1\]。所以递推公式是：buy\[i\] = Math.max\(rest\[i-1\] - prices\[i\], buy\[i-1\]\);
* 计算rest\[i\]的值，同样也分2种情况：前面到\(i-1\)为止最后一次操作为rest，或则为sell都可以。所以递推公式是：rest\[i\] = Math.max\(sell\[i-1\], rest\[i-1\]\)

就用上面3组递推公式一直计算即可，由于最后max profit 时，最后一次操作可能为rest \(cool down\)，也可能为sell，所以最后return的结果为 Math.max\(sell\[n-1\], rest\[n-1\]\)。

### Code 2\)

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if(n < 2) return 0;

        int[] sell = new int[n];
        int[] buy = new int[n];
        int[] rest = new int[n];
        rest[0] = 0;
        // 因为sell[0] 不可能为sell prices[0], 因为这是第一个price，必须先buy了才能sell
        // 所以表示不能取sell[0]的值即可，即把其赋值为 Integer.MIN_VALUE 即可
        sell[0] = Integer.MIN_VALUE;
        buy[0] = -prices[0];

        for(int i = 1; i < n; i ++) {
            sell[i] = Math.max(buy[i-1]+prices[i], sell[i-1]);
            buy[i] = Math.max(rest[i-1]-prices[i], buy[i-1]);
            rest[i] = Math.max(sell[i-1], rest[i-1]);
        }
        return Math.max(sell[n-1], rest[n-1]);
    }
}
```



