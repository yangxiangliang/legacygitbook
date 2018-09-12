# 767. Reorganize String

> Given a string`S`, check if the letters can be rearranged so that two characters that are adjacent to each other are not the same.
>
> If possible, output any possible result.  If not possible, return the empty string.
>
> **Example 1:**
>
> ```
> Input: S = "aab"
> Output: "aba"
> ```
>
> **Example 2:**
>
> ```
> Input: S = "aaab"
> Output: ""
> ```

### 思路

1. 分析题目，出现次数最多的character最容易adjacent to each other，所以要把它们分开，greedy的思想。
2. 首先需要统计每个character出现的频率。然后用max heap 维护当前剩下的char中频率最高的char。注意当前插入结果的char在下一次是不能插入的，所以从heap poll 出来的当前频率最高的char，插入结果后，不要马上又放回heap中。等“下一次”插入了不同元素后再重新放入 heap中。

### Code

```java
class Solution {
    class Pair {
        char c;
        int count;
        public Pair(char c, int count) {
            this.c = c;
            this.count = count;
        }
    }

    public String reorganizeString(String S) {
        HashMap<Character, Integer> map = new HashMap<>();
        for(char c : S.toCharArray()) map.put(c, map.getOrDefault(c, 0) + 1);
        Queue<Pair> heap = new PriorityQueue<Pair>(new Comparator<Pair>(){
            public int compare(Pair p1, Pair p2) { return p2.count - p1.count;}
        });

        StringBuilder rst = new StringBuilder();
        for(char c : map.keySet()) heap.add(new Pair(c, map.get(c)));
        Pair last = null;
        while(!heap.isEmpty()) {
            Pair cur = heap.poll();
            rst.append(cur.c);
            cur.count --;
            if(last != null && last.count > 0) heap.add(last);
            last = cur;
        }
        if(last != null && last.count > 0) return "";
        return rst.toString();
    }
}
```



