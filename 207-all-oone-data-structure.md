# 432. All O\`one Data Structure

> Implement a data structure supporting the following operations:
>
> 1. Inc\(Key\) - Inserts a new key with value 1. Or increments an existing key by 1. Key is guaranteed to be a **non-empty** string.
>
> 2. Dec\(Key\) - If Key's value is 1, remove it from the data structure. Otherwise decrements an existing key by 1. If the key does not exist, this function does nothing. Key is guaranteed to be a **non-empty** string.
>
> 3. GetMaxKey\(\) - Returns one of the keys with maximal value. If no element exists, return an empty string "".
>
> 4. GetMinKey\(\) - Returns one of the keys with minimal value. If no element exists, return an empty string "".
>
> Challenge: Perform all these in O\(1\) time complexity.

### 思路

1. 这里需要时间O\(1\)，就容易想到 set\(\) 数据结构。因为需要记录key和其对应的次数，一开始容易想到是不是采用HashMap来存key和其对应的value值，但是这样很难记录value的max, min 值。不能用O\(1\)时间找出max, min值对应的key。
2. 那么这里想除了用set，把value相同的key值放入同一个set中，当Inc\(key\)的时候，那对应set中的key去掉，放入对应更大value的set中。如果Dec\(key\)的话，就把当前set中的key去掉，放入对应更小value的set中，如果value 等于1，就把key“彻底去掉”。所以还是需要一个总的HashMap&lt;&gt;记录每个key和其对应的value关系。
3. 上面可以看出来，其实这个题目很像是"Bucket Sort"的变型，每个set就相当于一个bucket，里面是所以value对应的key值。Inc\(key\)就是把key从当前bucket中拿出来放入下一个对应更大值的bucket，Dec\(key\) 就是把key从当前bucket中拿出来放入前一个更小值的bucket。现在的问题就是怎么把这些buckets都放入一个结构中，然后让每次找到MaxValue,MinValue都是O\(1\)。一开始想到把buckets和其对应的value放入TreeMap中，但是TreeMap中的每次插入也不是O\(1\)。然后想到可不可以把buckets都按照value sort的顺序连起来，找max value 就找连起来的队尾的那个bucket，找min value就找连起来的队伍头的那个bucket。这里自然就想到了可不可以用LinkedList来解决，single linkedList不够，因为Dec\(key\)的时候，如果key的value变为\(value-1\)，没有（value-1）对应的bucket，要把新的bucket放在value对应的bucket前面，快速找bucket前一个bucket的话就需要double LinkedList。所以就把bucket作为double LinkedList，然后把每个bucket连接起来，再加上dummy head，dummy tail来方查找max value, min value。

### Code

```java
class AllOne {
    
    class Bucket {
        int count;
        HashSet<String> set;
        Bucket next;
        Bucket last;
        
        public Bucket(int count) {
            this.count = count;
            this.set = new HashSet();
        }
    }
    
    Bucket head;
    Bucket tail;
    HashMap<String, Integer> map;
    HashMap<Integer, Bucket> buckets;

    /** Initialize your data structure here. */
    public AllOne() {
        map = new HashMap();
        buckets = new HashMap();
        head = new Bucket(0);
        tail = new Bucket(0);
        head.next = tail;
        tail.last = head;
    }
    
    /** Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    public void inc(String key) {
        if(map.containsKey(key)) {
            int count = map.get(key);
            map.put(key, count+1); // update count value for key
            Bucket bucket = buckets.get(count);
            bucket.set.remove(key);
            
            // add key into (count+1) bucket
            if(buckets.containsKey(count+1)) buckets.get(count+1).set.add(key);
            else {
                // move newBucket(count+1) to position after bucket(count)
                Bucket newBucket = new Bucket(count+1);
                newBucket.next = bucket.next;
                newBucket.last = bucket;
                bucket.next = newBucket;
                newBucket.next.last = newBucket;
                newBucket.set.add(key);
                buckets.put(count+1, newBucket); // add bucket into map
            }
            
            // check if size of bucket for count is already 0
            if(bucket.set.size() == 0) {
                buckets.remove(count);
                // remove bucket
                bucket.last.next = bucket.next;
                bucket.next.last = bucket.last;
            }
        } else {
            map.put(key, 1);
            if(buckets.containsKey(1)) buckets.get(1).set.add(key);
            else {
                // add newBucket(count == 1) to position after head bucket
                Bucket newBucket = new Bucket(1);
                newBucket.next = head.next;
                newBucket.last = head;
                head.next = newBucket;
                newBucket.next.last = newBucket;
                newBucket.set.add(key);
                buckets.put(1, newBucket); // add bucket into map
            }
        }
    }
    
    /** Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    public void dec(String key) {
        if(map.containsKey(key) == false) return;
        int count = map.get(key);
        if(count == 1) map.remove(key);
        else map.put(key, count-1); // update count value for key
        Bucket bucket = buckets.get(count);
        bucket.set.remove(key);

        // add key into (count-1) bucket
        if(buckets.containsKey(count-1)) buckets.get(count-1).set.add(key);
        else if (count > 1) {
            // move newBucket(count-1) to position before bucket(count)
            Bucket newBucket = new Bucket(count-1);
            newBucket.next = bucket;
            newBucket.last = bucket.last;
            bucket.last = newBucket;
            newBucket.last.next = newBucket;
            newBucket.set.add(key);
            buckets.put(count-1, newBucket); // add bucket into map
        }

        // check if size of bucket for count is already 0
        if(bucket.set.size() == 0) {
            buckets.remove(count);
            // remove bucket
            bucket.last.next = bucket.next;
            bucket.next.last = bucket.last;
        }
    }
    
    /** Returns one of the keys with maximal value. */
    public String getMaxKey() {
        if(tail.last == head) return "";
        return buckets.get(tail.last.count).set.iterator().next();
    }
    
    /** Returns one of the keys with Minimal value. */
    public String getMinKey() {
        if(head.next == tail) return "";
        return buckets.get(head.next.count).set.iterator().next();
    }
}

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne obj = new AllOne();
 * obj.inc(key);
 * obj.dec(key);
 * String param_3 = obj.getMaxKey();
 * String param_4 = obj.getMinKey();
 */
```



