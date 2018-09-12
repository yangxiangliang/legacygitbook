# 347. Top K Frequent Elements

> Given a non-empty array of integers, return the **k **most frequent elements.
>
> For example,
>
> Given \[1, 1, 1, 2, 2, 3\] and k = 2, return \[1, 2\].
>
> **Note:**
>
> 1. You may assume k is always valid, i &lt;= k &lt;= number of unique elements.
> 2. Your algorithm's time complexity **must be** better than O\(n \* log\(n\)\)，when n is the array's size.

### 思路

1. 因为这里要求complexity 优于 O\(n \* log\(n\)\)，所有常规用heap不行。排序中复杂度优于这个就考虑buckets sort了。
2. 首先当然要统计每个元素和其出现的频率。记录出现最多的频率max，这个决定了buckets的个数。每个buckets里面就放对应频率的元素即可。
3. 因为对应某个频率的元素可能有好几个，可以考虑用List&lt;Integer&gt; 来store，buckets也需要List，所以总的buckets可以用List&lt;List&lt;Integer&gt;&gt; 来表示。里面包含了max frequency 个 List&lt;Integer&gt;。

### Code

```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap(); // 统计元素以及其对应的频率
        int max = 0;
        for(int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
            max = Math.max(max, map.get(num));
        }

        List<List<Integer>> buckets = new ArrayList();
        for(int i = 0; i < max; i ++) buckets.add(new ArrayList());
        for(int num : map.keySet()) {
            // 把元素加入其对应的buckets中
            buckets.get(map.get(num) - 1).add(num);
        }

        List<Integer> rst = new ArrayList();
        for(int i = max - 1; i >= 0; i --) {
            for(int num : buckets.get(i)) {
                rst.add(num);
                if(rst.size() == k) return rst;
            }
        }
        return rst;
    }
}
```



