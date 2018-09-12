# 753. Cracking the Safe

> There is a box protected by a password. The password is`n`digits, where each letter can be one of the first`k`digits`0, 1, ..., k-1`.
>
> You can keep inputting the password, the password will automatically be matched against the last`n`digits entered.
>
> For example, assuming the password is`"345"`, I can open it when I type`"012345"`, but I enter a total of 6 digits.
>
> Please return any string of minimum length that is guaranteed to open the box after the entire string is inputted.
>
> **Example 1:**
>
> ```
> Input: n = 1, k = 2
> Output: "01"
> Note: "10" will be accepted too.
> ```
>
> **Example 2:**
>
> ```
> Input: n = 2, k = 2
> Output: "00110"
> Note: "01100", "10011", "11001" will be accepted too.
> ```

### 思路

1. 自己手动写一些列子后，知道需要找到minimum length的答案，则需要对于string a 而言，利用a的后面\(n-1\)长度的substring，然后在a后面加一个所需的digit来得到下一个密码code，这样最后组成的string是包含所有密码组合的minimum length。需要证明的话，可以参考 [Video ](https://www.youtube.com/watch?v=iPLQgXUiU14)

2. 因为当前密码string而言，取其后面\(n-1\)长度的substring，后面加什么digit的char来得到下一个密码不太清楚，只能用DFS来尝试。然后下面需要决定DFS的退出条件，怎么才知道得到了所有的密码组合。因为容易得出一共有 \(k ^ n\) 个密码组合。所有可以用一个set记录已经得到的密码，如果set的size等于 \(k ^ n\)，则说明DFS结束，得到了所有的密码组合。

### Code

```java
class Solution {
    public String crackSafe(int n, int k) {
        StringBuilder sb = new StringBuilder();
        int goal = (int) Math.pow(k, n);
        // initialize string with all 0
        for(int i = 0; i < n; i ++) sb.append('0');

        HashSet<String> visited = new HashSet();
        visited.add(sb.toString());
        dfs(n, k, goal, sb, visited);
        return sb.toString();
    }

    // return boolean 表示从当前条件下是否能得到所有的密码组合
    private boolean dfs(int n, int k, int goal, StringBuilder sb, HashSet<String> visited) {
        if(visited.size() == goal) return true;

        String pre = sb.substring(sb.length() - n + 1, sb.length());
        for(int i = 0; i < k; i ++) {
            String next = pre + i;
            if(!visited.contains(next)) {
                sb.append(i);
                visited.add(next);
                
                // 因为不知道当前sb后面加上 i char后，可否通过DFS得到所有密码组合，所有这里用DFS尝试
                // 如果不行，则remove掉添加的 i char 和 visited里面新添加的密码
                if(dfs(n, k, goal, sb, visited)) return true;
                else {
                    visited.remove(next);
                    sb.deleteCharAt(sb.length() - 1);
                }
            }
        }
        return false;
    }
}
```



