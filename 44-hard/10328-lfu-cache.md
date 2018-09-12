# 460. LFU Cache

> Design and implement a data structure for [Least Frequently Used \(LFU\)](https://en.wikipedia.org/wiki/Least_frequently_used) cache. It should support the following operations:`get`and`put`.
>
> `get(key)`- Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
> `put(key, value)`- Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least frequently used item before inserting a new item. For the purpose of this problem, when there is a tie \(i.e., two or more keys that have the same frequency\), the least **recently **used key would be evicted.
>
> **Follow up:**
>
> Could you do both operations in **O\(1\) **time complexity?
>
> **Example:**
>
> ```
> LFUCache cache = new LFUCache( 2 /* capacity */ );
> cache.put(1, 1);
> cache.put(2, 2);
> cache.get(1);       // returns 1
> cache.put(3, 3);    // evicts key 2
> cache.get(2);       // returns -1 (not found)
> cache.get(3);       // returns 3.
> cache.put(4, 4);    // evicts key 1.
> cache.get(1);       // returns -1 (not found)
> cache.get(3);       // returns 3
> cache.get(4);       // returns 4
> ```

### 思路

因为这里要求complexity是 O\(1\) ，容易想到会用HashMap来处理。首先可以用HashMap记录key, value的pair来快速look up。由于这里需要 least frequently used，所以想到还可以用一个HashMap记录每个key对应被used的次数。关键在capacity 满以后，怎么选择least frequently used的元素中又是least recently used的那个，然后将其remove掉。想到为了 O\(1\) 快速remove和add，可以用一个HashMap，key是used的次数，然后value是set记录当前该used的次数的所有元素。因为当used的次数相同的时候，需要remove掉least used key，所以当加元素入set的时候，最好还有顺序，即后加的元素在后面，然后每次remove掉最头的元素即可，这里就可以用到 **LinkedHashSet** 来处理。

### Code

```java
public class LFUCache {
    HashMap<Integer, Integer> values; // 记录 key, value pair的值
    HashMap<Integer, Integer> counts; // 记录 key和其被used的次数
    HashMap<Integer, LinkedHashSet<Integer>> lists; // 记录相同被used的次数的所有元素的keys
    int cap;
    int min; // 记录当前keys中被used的最少的次数

    public LFUCache(int capacity) {
        values = new HashMap();
        counts = new HashMap();
        lists = new HashMap();
        cap = capacity;
        min = 0;
    }

    public int get(int key) {
        if(!values.containsKey(key)) return -1;

        int count = counts.get(key);
        counts.put(key, count+1);

        lists.get(count).remove(key);
        // 如果该元素就是被used的最少次数的那个，而且该次数的元素只有它一个
        // 那么被used的最少次数需要 +1,因为该元素被used的次数 +1 了
        if(count == min && lists.get(count).size() == 0) {
            min ++;
        }
        if(!lists.containsKey(count+1)) {
            lists.put(count+1, new LinkedHashSet());
        }
        lists.get(count+1).add(key);
        return values.get(key);
    }

    public void put(int key, int value) {
        if(cap <= 0) return;

        if(values.containsKey(key)) {
            values.put(key, value);
            // 因为put()也算被used了一次，所以做一次get()操作，增加一次其被used的次数
            get(key);
            return;
        }
        
        // 检查capacity是否够，是否需要remove掉元素
        if(values.size() == cap) {
            // remove掉min对应的LinkedHashSet()的第一个元素
            int remove = lists.get(min).iterator().next();
            values.remove(remove);
            counts.remove(remove);
            lists.get(min).remove(remove);
        }

        values.put(key, value);
        counts.put(key, 1);
        min = 1; // 注意因为key此时被use了1次，所以min肯定为1
        if(!lists.containsKey(1)) {
            lists.put(1, new LinkedHashSet());
        }
        lists.get(1).add(key);
    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```



