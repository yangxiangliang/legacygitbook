# 281. Zigzag Iterator

> Given two 1d vectors, implement an iterator to return their elements alternately.
>
> For example, given two 1d vectors:   v1 = \[1, 2\]   v2 = \[3, 4, 5, 6\]
>
> By calling _next_ repeatedly until _hasNext_ returns `false`, the order of elements returned by _next_ should be: `[1, 3, 2, 4, 5, 6]`.
>
> **Follow up**: What if you are given`k`1d vectors? How well can your code be extended to such cases?

### 思路

**1\)**  一开始最容易想到的就是用两个pointer记录分别遍历到v1和v2的元素位置，然后一个flag记录当前应该遍历v1还是v2的元素。但是这种方法很难解决 **follow up**的问题，当有k个 1d vectors时候，很难用同样的code解决。

**2\)** 题目和iterator有关，想到可不可以直接利用v1和v2的iterator。可以用一个queue或则list，一开始加入v1, v2的iterator，然后从头开始取出v1的iterator，return该iterator的next element，此时 如果该iterator还有next元素，那么把该iterator又一次加入队尾。如此往复，便可以到达题目要求。该方法容易scale到K 1d vectors的情况。

### Code 1\)

```java
public class ZigzagIterator {
    int index1 = 0;
    int index2 = 0;
    boolean first = true; // indicate 我们应该return v1的元素还是v2的元素
    List<Integer> v1 = new ArrayList();
    List<Integer> v2 = new ArrayList();

    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        this.v1 = v1;
        this.v2 = v2;
    }

    public int next() {
        int rst = 0;
        if (first) {
            if(index1 < v1.size()) {
                rst = v1.get(index1);
                index1 ++;
            } else {
                rst = v2.get(index2);
                index2 ++;
            } 
        } else {
            if(index2 < v2.size()) {
                rst = v2.get(index2);
                index2 ++;
            } else {
                rst = v1.get(index1);
                index1 ++;
            }
        }
        first = !first;
        return rst;
    }

    public boolean hasNext() {
        return index1 < v1.size() || index2 < v2.size();
    }
}
```

### Code 2\)

```java
public class ZigzagIterator {
    List<Iterator> list; // 注意list中包含的是各个list的 iterator

    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        list = new ArrayList();
        if(v1.size() > 0) list.add(v1.iterator());
        if(v2.size() > 0) list.add(v2.iterator());
    }

    public int next() {
        Iterator removed = list.remove(0);
        int rst = (Integer)removed.next();
        if(removed.hasNext()) list.add(removed);
        return rst;
    }

    public boolean hasNext() {
        return list.size() > 0;
    }
}
```



