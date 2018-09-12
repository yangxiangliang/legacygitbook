# 403. Frog Jump

> A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.
>
> Given a list of stones' positions \(in units\) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.
>
> If the frog's last jump was k units, then its next jump must be either k- 1, k, or k+ 1 units. Note that the frog can only jump in the forward direction.
>
> **Note:**
>
> * The number of stones is &gt;= 2 and is &lt; 1,100.
> * Each stone's position will be a non-negative integer &lt; 2^31
> * The first stone's position is always 0
>
> **Example 1:**
>
> ```
> [0,1,3,5,6,8,12,17]
>
> There are a total of 8 stones.
> The first stone at the 0th unit, second stone at the 1st unit,
> third stone at the 3rd unit, and so on...
> The last stone at the 17th unit.
>
> Return true. The frog can jump to the last stone by jumping 
> 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 
> 2 units to the 4th stone, then 3 units to the 6th stone, 
> 4 units to the 7th stone, and 5 units to the 8th stone.
> ```
>
> **Example 2:**
>
> ```
> [0,1,2,3,4,8,9,11]
>
> Return false. There is no way to jump to the last stone as 
> the gap between the 5th and 6th stone is too large.
> ```

### 思路

1. 一开始那道题看不太出来思路，就从一开始的石头开始跳，看有没有什么规律。每从i步跳到\(i+1\)步时，知道跳的步数K，通过步数K便知道能否往下跳到\(i+2\)个石头，即看\(i+1\), \(i+2\)之间的gap是不是\[k-1, k+1\]之间的。
2. 这里注意的是到达\(i+1\)石头的时候，可能不是从i跳过去的，可能是从\(i-1\)跳过去，那么可能有好几个K值都能达到\(i+1\)石头，所以从\(i+1\)石头往下跳的时候，可能有好几个K值可以选择。所以需要一个HashMap记录stone\[i\] 和 能到达该stone的 K值的集合。看能不能跳到终点，就是看HashMap中stone\[end\]有没有对应的K值即可。

### Code

```java
class Solution {
    public boolean canCross(int[] stones) {
        if(stones[1] > 1) return false; // corner case, stones[1] has to be 1 for valid jump
        Map<Integer, HashSet<Integer>> map = new HashMap();
        for(int stone : stones) map.put(stone, new HashSet());
        map.get(0).add(0);
        for(int stone : stones) {
            for(int k : map.get(stone)) {
                for(int step = k - 1; step <= k + 1; step++) {
                    // 只考虑 step > 0的情况，因为不能往后跳，原地跳也没意义
                    if(step > 0 && map.containsKey(step + stone)) map.get(step + stone).add(step);
                }
            }
        }
        return !map.get(stones[stones.length-1]).isEmpty();
    }
}
```

### 思路 2）

1. 仔细想想跳的过程，从i 往下跳，其实可以看做2步: （1）从i开始跳\[k-1, k+1\] 步有没有下一个石头，不一定是和 i 紧挨着的；\(2\) 从下一个石头能否跳到最后的石头。这里就是有recursion DFS的感觉，可以DFS来做，在recursion函数中pass 入跳到当前的position和K值，看能不能继续往下跳直到跳到最后。
2. 为了避免重复计算，可以用一个HashMap&lt;&gt; 记录是否能从当前position跳到最后成功与否\(True OR False\)。**\(坑\)** 这个HashMap的key不能是integer，光是stones的位置，因为一个stones可能有多种方法跳到该位置，K值不同，有些方法可以让其跳到最后，有些则不行，需要区分开来。所以可以用个String 包含 position位置和跳到position的K值2个信息就能保证其unique。

### Code

```java
class Solution {
    public boolean canCross(int[] stones) {
        HashMap<String, Boolean> memo = new HashMap();
        return canCross(stones, 0, 0, memo);
    }

    private boolean canCross(int[] stones, int pos, int k, HashMap<String, Boolean> map) {
        String key = "pos" + pos + "k" + k;
        if(map.containsKey(key)) return map.get(key);

        for(int i = pos + 1; i < stones.length; i ++) {
            int gap = stones[i] - stones[pos];
            // 可能直接跳到后面隔一个的石头上，所以continue查看下一个石头即可
            if(gap < k - 1) continue;
            if(gap > k + 1) {
                map.put(key, false);
                return false;
            }
            if(canCross(stones, i, gap, map)) {
                map.put(key, true);
                return true;
            }
        }
        map.put(key, pos == stones.length - 1);
        return map.get(key);
    }
}
```



