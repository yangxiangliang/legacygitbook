# 380. Insert Delete GetRandom O\(1\)

> Design a data structure that supports all following operations inaverage **O\(1\) **time
>
> 1. `insert(val)`: Inserts an item val to the set if not already present.
> 2. `remove(val)`: Removes an item val from the set if present.
> 3. `getRandom`: Returns a random element from current set of elements. Each element must have the **same probability **of being returned.
>
> **Example: **
>
> ```
> // Init an empty set.
> RandomizedSet randomSet = new RandomizedSet();
>
> // Inserts 1 to the set. Returns true as 1 was inserted successfully.
> randomSet.insert(1);
>
> // Returns false as 2 does not exist in the set.
> randomSet.remove(2);
>
> // Inserts 2 to the set, returns true. Set now contains [1,2].
> randomSet.insert(2);
>
> // getRandom should return either 1 or 2 randomly.
> randomSet.getRandom();
>
> // Removes 1 from the set, returns true. Set now contains [2].
> randomSet.remove(1);
>
> // 2 was already in the set, so return false.
> randomSet.insert(2);
>
> // Since 2 is the only number in the set, getRandom always return 2.
> randomSet.getRandom();
> ```

### 思路

由于需要O\(1\)的insert和remove，容易想到需要数据结构HashMap或则HashSet之类的。但是这里 **GetRandom** 怎么在O\(1\)处理，可以想到将元素都放到arrayList中，然后用list.get\(random.nextInt\(list.size\(\)\)\) 可以得到符合要求的random元素。由于在remove value的时候，也需要将value从arrayList中remove掉，所以HashMap中的key是value的值，map中的value应该是该key对应在arrayList中的index。

### 注意

* ArrayList中remove掉index处的元素的complexity是O\(n\)，而不是O\(1\)。但是有例外的情况，就是remove掉最后1个元素的complexity是O\(1\)。所以这里remove元素时，将arrayList的最后1个元素替换到需要remove的元素的位置，然后remove list中的最后一个element。

### Code

```java
public class RandomizedSet {
    ArrayList<Integer> list;
    HashMap<Integer, Integer> map;
    java.util.Random random;
    
    /** Initialize your data structure here. */
    public RandomizedSet() {
        list = new ArrayList();
        map = new HashMap();
        random = new java.util.Random();
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        if(map.containsKey(val)) return false;
        list.add(val);
        map.put(val, list.size() - 1); // index of val in arraylist
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        if(!map.containsKey(val)) return false;
        int index = map.get(val);
        int last = list.get(list.size() - 1);
        list.set(index, last);
        map.put(last, index); // 此时last对应的在arraylist中的index也变化了，需要update hashMap
        list.remove(list.size() - 1);
        map.remove(val);
        return true;
    }
    
    /** Get a random element from the set. */
    public int getRandom() {
        return list.get(random.nextInt(list.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```



