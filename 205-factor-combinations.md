# 254. Factor Combinations

> Numbers can be regarded as product of its factors. For example,
>
> ```
> 8 = 2 x 2 x 2;
>   = 2 x 4.
> ```
>
> Write a function that takes an integer n and return all possible combinations of its factors.
>
> **Note:**
>
> 1. You may assume that _n_ is always positive.
> 2. Factors should be greater than 1 and less than _n_ .
>
> **Examples:**  
> input: 1
>
> output: \[\]
>
> input: 37
>
> output: \[\]
>
> input: 12
>
> output:
>
> ```
> [
>   [2, 6],
>   [2, 2, 3],
>   [3, 4]
> ]
> ```
>
> input: 32
>
> ```
> [
>   [2, 16],
>   [2, 2, 8],
>   [2, 2, 2, 4],
>   [2, 2, 2, 2, 2],
>   [2, 4, 4],
>   [4, 8]
> ]
> ```

### 思路

1. 首先容易想到找factor的常规方法，就是用n % i == 0，如果 0 &lt; i &lt; n，那么 i 就是n的一个factor。那么比较暴力的就是对于n，先来个\(for i = 1; i &lt; n; i ++\) 来找其factor，然后用n 除以该factor，得到一个较小的值，再找该较小值的factor，这样一步一步往下找，找到无法有factor为止。因为就是一步一步往下走，往下走分不同的路径，形态就很像是Backtracking，用到backtracking的思想。
2. **\(坑\)** 这里主要是怎么避免找到duplicate的结果。比如8 = \[2, 4\]，不把\[4, 2\] 加入到结果中。那么我们需要把结果List&lt;Integer&gt; 中的factor都按照一定顺序排列来找即可，像题目例子中的将factors从小到大排列即可。那么我们需要将每次找到的factor 当做parameter pass到下一层函数中，下一层函数中找到的factor至少不能小于上一层找到 的factor即可。这样就保证最后输出 的factors是按照non-descending 顺序排列的。

### Code 1\)

```java
class Solution {
    public List<List<Integer>> getFactors(int n) {
        List<List<Integer>> rst = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        for(int i = 2; i < n ; i ++) {
            if (n % i == 0) {
                path.add(i);
                getFactorsHelper(i, n / i, rst, path);
                path.remove(path.size()-1);
            }
        }
        return rst;
    }

    // need to find any factors of n bigger than bound
    private void getFactorsHelper(int bound, int n, List<List<Integer>> rst, List<Integer> path) {
        if (n < bound) return; // (关键)avoid duplicate results

        // path.add(n) 肯定是符合要求的一个答案，所以先把其加入结果中
        path.add(n);
        // initialize new arrayList to add it to final result to avoid any operations to path later affects final result
        rst.add(new ArrayList<>(path));
        path.remove(path.size()-1);

        for(int i = bound; i < n; i ++) {
            if(n % i == 0) {
                path.add(i);
                getFactorsHelper(i, n / i, rst, path);
                path.remove(path.size()-1);
            }
        }
    }
}
```

### Code 2\)

思路和上面一样，只是用一个global的hashMap作为memo来做memorization。

```java
class Solution {
    // use for memorization
    HashMap<Integer, List<List<Integer>>> map = new HashMap();

    public List<List<Integer>> getFactors(int n) {
        if(map.containsKey(n)) return map.get(n);
        List<List<Integer>> rst = new ArrayList();

        for(int i = 2; i < n; i ++) {
            if(n % i == 0) {
                for(List<Integer> list : getFactors( n / i)) {
                    // 避免重复的list在结果中，this line 保证结果的list中的integer按照non-descending order排列
                    if( i <= list.get(0) ) {
                        // initialize新的list，不能在原来list上面add(0, i) 元素
                        // 因为会改变getFactors(n / i) 结果中的list
                        List<Integer> tmp = new ArrayList(list);
                        tmp.add(0, i);
                        rst.add(tmp);
                    }
                }

                // 因为每个函数只考虑factor > 1, factor < n的情况，所以getFactors(n/i)的结果factor中不包括(n/i)
                // 这里handle是[ i, n / i] 这种Corner case情况
                if(i <= n / i) {
                    List<Integer> tmp = new ArrayList();
                    tmp.add(i);
                    tmp.add(n / i);
                    rst.add(tmp);
                }
            }
        }
        map.put(n, rst);
        return rst;
    }
}
```



