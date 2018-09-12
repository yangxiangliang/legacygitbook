# 251. Flatten 2D Vector

> Implement an iterator to flatten a 2d vector.
>
> For example,
>
> Given 2d vector =
>
> ```
> [
>   [1,2],
>   [3],
>   [4,5,6]
> ]
> ```
>
> By calling _next_ repeatedly until  _hasNext_ returns false, the order of elements returned by_ next_ should be: `[1,2,3,4,5,6]`.
>
> **Follow up:**
>
> As an added challenge, try to code it using only [iterators in C++](http://www.cplusplus.com/reference/iterator/iterator/) or [iterators in Java](http://docs.oracle.com/javase/7/docs/api/java/util/Iterator.html).

### 思路

因为 Follow up 中说尽量就用 Java 中的 iterators，那么可以用2个iterator，一个是整个2d vector List&lt;List&lt;Integer&gt;&gt;的iterator，一个是当前正在遍历的List&lt;Integer&gt;的iterator。这样extra space就是O\(1\)

### Code 1\)

一开始没想到用2个iterator，还是用1个List&lt;List&lt;Integer&gt;&gt; list来记录所需要遍历的2d vector，然后index记录当前遍历的list&lt;Integer&gt;，这样的话该vector 2D的iterator需要用到 extra space

```java
public class Vector2D implements Iterator<Integer> {
    List<List<Integer>> list;
    Iterator<Integer> iterator;
    int index;

    public Vector2D(List<List<Integer>> vec2d) {
        index = 0;
        list = vec2d;
        iterator = null;
    }

    @Override
    public Integer next() {
        return iterator.next();
    }

    @Override
    public boolean hasNext() {
        if(iterator == null || !iterator.hasNext()) {
            while(index < list.size() && list.get(index).size() == 0) index ++;
            if(index == list.size()) return false;
            iterator = list.get(index++).listIterator();
        }
        return iterator.hasNext();
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i = new Vector2D(vec2d);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

### Code 2\)

用2个iterator，不需要用到 extra space

```java
public class Vector2D implements Iterator<Integer> {
    Iterator<List<Integer>> i;
    Iterator<Integer> j;

    public Vector2D(List<List<Integer>> vec2d) {
        i = vec2d.iterator();
    }

    @Override
    public Integer next() {
        return j.next();
    }

    @Override
    public boolean hasNext() {
        while((j == null || !j.hasNext()) && i.hasNext())
            j = i.next().iterator();
        return j != null && j.hasNext();
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i = new Vector2D(vec2d);
 * while (i.hasNext()) v[f()] = i.next();
 */
```



