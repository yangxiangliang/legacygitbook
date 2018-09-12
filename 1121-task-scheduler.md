# 621. Task Scheduler

> Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks.Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.
>
> However, there is a non-negative cooling interval **n **that means between two **same tasks**, there must be at least n intervals that CPU are doing different tasks or just be idle.
>
> You need to return the **least **number of intervals the CPU will take to finish all the given tasks.
>
> **Example 1:**
>
> ```
> Input: tasks = ["A","A","A","B","B","B"], n = 2
> Output: 8
> Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
> ```
>
> **Note:**
>
> 1. The number of tasks is in the range \[1, 10000\].
> 2. The integer n is in the range \[0, 100\].

### 思路

1. 这题目要求向一系列的intervals中插入task或则idle，相同的task的间隔至少是n。插入的要求只对相同的task起作用，因此想到出现frequency最高的task是最“危险”的，最容易打破规则的，所以想到是否可以优先解决出现frequency最高的task。
2. 比如 \[A, A, A, B, B, C, C\], n = 2 出现次数最多的A 3次, 我们先将A放入一个框架中"AXXAXXA....."， 这里”X“代表其他task或则idle，因为相同的A至少间隔 n=2 个，所以间隔2个”X“，最后的”......“表示可能有其他的task，也可能没有。然后只需要把其他task\(B, C\)插入A划分出来的区域中即可，这些区域已经被A划分为了间隔至少为n=2的，而且B,C都只出现2次，没有3次，所以第3个A后面也没有其他task。不难发现，这种情况下总的interval的数量\(n+1\) \* \(maxFrequency-1\) + numOfMaxFrequencyTasks
3. **注意：**除了上面的情况，容易漏掉一种情况，就是task的种类多于\(n+1\)，那么A划分的每一个区域中放不下每一种task。比如    \[A, A, A, B, B, C, C, D, D\], 这里不能用上面的公式，上面的方法算出来的结果是小于 tasks.length 的，显然最终结果是大于或则等于tasks.length。在这种情况下，也容易证明总的intervals的长度就是tasks.length。一开始还是按照\(2\)中的方法摆好A, "AXXAXXA..."，至少需要tasks.length = 9 个，所以至少"AXXAXXAXXX", 还剩下6个“X”的位置摆放剩下的6个tasks\(BBCCDD\)，很简单，把剩下6个中出现频率最高的B摆放进剩下的6个X中，即1st和4th个X，然后摆放C和D。这样肯定符合要求，因为就算把A拿掉，按照这样摆放B, C, D都是符合要求的，加入了A，只会让其间隔更大，更符合要求。
4. 综合上面讲的，所以最后答案需要在 \(2\)中的算是和 tasks.length 中取最大值 即可。

### Code

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        // Note: 一个就26个字母，不用hashMap了，用int[26]来记录task出现的次数即可。index = char - 'A' 表示即可
        int[] count = new int[26];
        int max = 0; // num for max frequency
        int numOfMax = 0; // num of tasks which has max frequenc
        for(char t: tasks) {
            int index = t - 'A';
            count[index] ++;
            if(count[index] > max) {
                max = count[index];
                numOfMax = 1;
            } else if (count[index] == max) numOfMax ++;
        }

        return Math.max((n+1) * (max-1) + numOfMax, tasks.length);
    }
}
```

### 思路 2）

1. 采用greedy的思想，统计每个task出现的频率，每次取出频率最多的task放入结果中。这里用priority queue 维护 task的频率，即max heap来维护。
2. 注意每次放完所有 priority queue中的task后，要看是否放了\(n + 1\)  个task，因为间隔为n个，所以放 \(n + 1\)个task，不够的需要用idle 补充上。

### Code 2\)

```java
class Solution {
    class Node {
        char val;
        int count;

        public Node(char _val, int _count) {
            val = _val;
            count = _count;
        }
    }

    public int leastInterval(char[] tasks, int n) {
        HashMap<Character, Integer> map = new HashMap<>();
        for(char c : tasks) map.put(c, map.getOrDefault(c, 0) + 1);

        Queue<Node> queue = new PriorityQueue<>(new Comparator<Node>(){
            public int compare(Node a, Node b) {return b.count - a.count;}
        });

        for(char c : map.keySet()) queue.offer(new Node(c, map.get(c)));
        int count = 0;
        while(!queue.isEmpty()) {
            int k = n + 1;
            List<Node> tmp = new ArrayList<>();

            while(k > 0 && queue.size() > 0) {
                Node cur = queue.poll();
                cur.count --;
                tmp.add(cur);
                count ++;
                k --;
            }

            for(Node i : tmp) {
                if(i.count > 0) queue.offer(i);
            }

            if(queue.isEmpty()) break; // 如果已经放完所有的task，直接break就行，不用后面 count += k 了
            count += k; // if k > 0, means we need to add k 个 idle
        }
        return count;
    }
}
```



