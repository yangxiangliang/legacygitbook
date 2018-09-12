# 128. Longest Consecutive Sequence

> Given an unsorted array of integers, find the length of the longest consecutive elements sequence.
>
> For example,  
> Given`[100, 4, 200, 1, 3, 2]`,  
> The longest consecutive elements sequence is`[1, 2, 3, 4]`. Return its length:`4`.
>
> Your algorithm should run in O\(n\) complexity.

### 思路

1\) 一开始想这个题目想复杂了，开始想用union find来做。用2个HashMap roots, count，roots用来记录每个num和其对应的root，比如num i 的root \(root1\) 应该和 （i-1）的root \(root2\) 相同，如果\(i-1\)在array中的话。如果这两个root不相同，那么需要将union root1, root2， 并且在记录该root下有多少个node的map中update node的个数。如题目中的例子，最后\[1, 2, 3, 4\]中的root都应该是1。

2\) 不用union find，而且上面的方法不是O\(n\), 直接用先用HashSet记录所有的Num，用hashSet是方便look up，而且可以avoid duplicates。然后遍历array中的所有num，用一个counter记录，如果发现set中有num-1, counter++，继续找num-2.......同理找set中是否有num+1, num+2...... 同时找过的num加入第二个hashSet&lt;Integer&gt; found，找过的num不重复遍历。这样complexity是O\(n\)

### Code 1\)

```java
public class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums.length == 0) return 0;
        HashMap<Integer, Integer> roots = new HashMap();
        HashMap<Integer, Integer> count = new HashMap();
        int max = 1;
        for(int i : nums) {
            roots.put(i, i);
            count.put(i, 1);
        }
        for(int i : roots.keySet()){
            if(roots.containsKey(i-1)) {
                int root1 = findRoot(roots, i-1);
                int root2 = findRoot(roots, i);
                // union 2 root
                if(root1 != root2) {
                    roots.put(root2, root1);
                    count.put(root1, count.get(root1) + count.get(root2));
                    max = Math.max(max, count.get(root1));
                }
            }
        }
        return max;
    }
    
    public int findRoot(HashMap<Integer, Integer> map, int p) {
        while(p != map.get(p)) p = map.get(p);
         return p;
    }
}
```

### Code 2\)

```java
public class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums.length == 0) return 0;
        HashSet<Integer> set = new HashSet();
        int max = 1;
        for(int i : nums) {
            set.add(i);
        }
        HashSet<Integer> found = new HashSet();
        for(int i = 0; i < nums.length; i ++){
            int tmp = nums[i];
            if(found.contains(tmp)) continue;
            
            found.add(tmp);
            int count = 1;
            while(set.contains(tmp+1)) {
                found.add(++tmp);
                count ++;
            }
            tmp = nums[i];
            while(set.contains(tmp-1)) {
                found.add(--tmp);
                count ++;
            }
            max = Math.max(count, max);
        }
        return max;
    }
}
```



