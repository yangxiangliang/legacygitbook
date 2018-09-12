# 274. H-Index

> Given an array of citations \(each citation is a non-negative integer\) of a researcher, write a function to compute the researcher's h-index.
>
> According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index h if h of his/her N papers have **at least **h citations each, and the other N − h papers have **no more than **h citations each."
>
> For example, given`citations = [3, 0, 6, 1, 5]`, which means the researcher has`5`papers in total and each of them had received`3, 0, 6, 1, 5`citations respectively. Since the researcher has`3`papers with **at least**`3`citations each and the remaining two with **no more than**`3`citations each, his h-index is`3`.
>
> **Note**: If there are several possible values for`h`, the maximum one is taken as the h-index.

### 思路 1\)

想到的是普通方法，就是先sort一遍input array，然后在sorted的数组中，从大到下，找valid的h值，然后更新h值。但是复杂度是：O\(N \* log\(N\)\)

### Code 1\)

```java
public class Solution {
    public int hIndex(int[] citations) {
        Arrays.sort(citations);
        int h = 0;
        for(int i = citations.length - 1; i >= 0; i --) {
            int tmp = citations.length - i; // potential的h 值
            if((i == 0 || citations[i-1] <= tmp) && citations[i] >= tmp) h = tmp;
        }
        return h;
    }
}
```

### 思路 2）

这道题目的关键就是找到信息"array中大于某个值的元素的个数"，可以想到bucket sort的思想。假定input citations的长度为length，用另外1个长度为\(length + 1\)的int\[\] buckets，buckets\[i\]表示citations的值等于i的元素个数，如果citations\[i\]大于等于length，那么都算在buckets\[length\]这个bucket中。因为需要找最大的h值，那么从buckets的index 大到小，用1个int count记录大于等于该index的元素个数，如果count &gt;= index，那么该index就是所寻找的h index。复杂度是O\(N\)

### Code 2\)

```java
public class Solution {
    public int hIndex(int[] citations) {
        int length = citations.length;
        int[] buckets = new int[length + 1];
        for(int num : citations) {
            if(num >= length) buckets[length] ++;
            else buckets[num] ++;
        }

        int count = 0;
        for(int i = buckets.length - 1; i >= 0; i --) {
            count += buckets[i];
            if(count >= i) return i;
        }
        return 0;
    }
}
```



