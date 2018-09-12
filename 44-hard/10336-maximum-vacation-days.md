# 568. Maximum Vacation Days

> LeetCode wants to give one of its best employees the option to travel among **N **cities to collect algorithm problems. But all work and no play makes Jack a dull boy, you could take vacations in some particular cities and weeks. Your job is to schedule the traveling to maximize the number of vacation days you could take, but there are certain rules and restrictions you need to follow.
>
> **Rules and restrictions:**
>
> 1. You can only travel among **N **cities, represented by indexes from 0 to N-1. Initially, you are in the city indexed 0 on
>    **Monday**.
> 2. The cities are connected by flights. The flights are represented as a **N\*N **matrix \(not necessary symmetrical\), called **flights **representing the airline status from the city i to the city j. If there is no flight from the city i to the city j,     **flights\[i\]\[j\] = 0**; Otherwise, **flights\[i\]\[j\] = 1**. Also, **flights\[i\]\[i\] = 0 **for all i.
> 3. You totally have **K **weeks \(**each week has 7 days**\) to travel. You can only take flights at most once **per day **and can only take flights on each week's **Monday **morning. Since flight time is so short, we don't consider the impact of flight time.
> 4. For each city, you can only have restricted vacation days in different weeks, given an **N\*K **matrix called **days **representing this relationship. For the value of **days\[i\]\[j\]**, it represents the maximum days you could take vacation in the city **i **in the week **j**.
>
> You're given the **flights **matrix and **days **matrix, and you need to output the maximum vacation days you could take during
>
> **K **weeks.
>
> **Example 1:**
>
> ```
> Input:flights = [[0,1,1],[1,0,1],[1,1,0]], days = [[1,3,1],[6,0,3],[3,3,3]]
> Output: 12
> Explanation: 
> Ans = 6 + 3 + 3 = 12. 
>
> One of the best strategies is:
> 1st week : fly from city 0 to city 1 on Monday, and play 6 days and work 1 day. 
> (Although you start at city 0, we could also fly to and start at other cities since it is Monday.) 
> 2nd week : fly from city 1 to city 2 on Monday, and play 3 days and work 4 days.
> 3rd week : stay at city 2, and play 3 days and work 4 days.
> ```
>
> **Example 2:**
>
> ```
> Input:flights = [[0,0,0],[0,0,0],[0,0,0]], days = [[1,1,1],[7,7,7],[7,7,7]]
> Output: 3
> Explanation: 
> Ans = 1 + 1 + 1 = 3. 
>
> Since there is no flights enable you to move to another city, you have to stay at city 0 for the whole 3 weeks. 
> For each week, you only have one day to play and six days to work. 
> So the maximum number of vacation days is 3.
> ```
>
> **Example 3:**
>
> ```
> Input:flights = [[0,1,1],[1,0,1],[1,1,0]], days = [[7,0,0],[0,7,0],[0,0,7]]
> Output: 21
> Explanation:
> Ans = 7 + 7 + 7 = 21
>
> One of the best strategies is:
> 1st week : stay at city 0, and play 7 days. 
> 2nd week : fly from city 0 to city 1 on Monday, and play 7 days.
> 3rd week : fly from city 1 to city 2 on Monday, and play 7 days.
> ```

### 思路

1. 一开始拿到题目发现有点乱，仔细分析发现current week在city i的max vacation days 和 previous week 在哪个city 相关。即现在的max vacation days 依赖于前一周各个city的max vacation days。典型应该考虑 DP 解决。
2. 用 int\[n\] dp 来记录当前week如果待在各个city的max vacation days。那么传递函数：dp\[j\] 表示所有能到city j 的city 在previous week的max vacation days + city j 在current week的vacation days，所有这些可能结果中的最大值即是current week 的 dp\[j\] 值

### Code

```java
class Solution {
    public int maxVacationDays(int[][] flights, int[][] days) {
        int n = flights.length;
        int k = days[0].length;
        // dp[i] means max vacation days if currently in city i
        int[] dp = new int[n];

        // initialize for first week
        for(int i = 0; i < n; ++i) {
            if (i == 0 || flights[0][i] == 1) dp[i] = days[i][0];
            else dp[i] = -1; // means we cannot get city i at first week
        }

        for(int i = 1; i < k; ++i) {
            // copy of max vacation days till previous week for every city
            int[] tmp = dp.clone();

            // j means which city we stays at current week
            for(int j = 0; j < n; ++j) {
                // pre means which city we stays at previous week
                for(int pre = 0; pre < n; ++pre) {
                    // means we cannot arrive city pre at previous week
                    if(tmp[pre] == -1) continue;
                    if (pre == j || flights[pre][j] == 1) dp[j] = Math.max(dp[j], tmp[pre] + days[j][i]);
                }
            }
        }

        int rst = 0;
        for(int tmp : dp) rst = Math.max(rst, tmp);
        return rst;
    }
}
```



