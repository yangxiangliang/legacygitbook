# 275. H-Index II

> Given an array of citations **sorted in ascending order**\(each citation is a non-negative integer\) of a researcher, write a function to compute the researcher's h-index.
>
> According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index h if h of his/her N papers have **at least **h citations each, and the other N − h papers have **no more than **h citations each."
>
> **Example:**
>
> ```
> Input: citations = [0,1,3,5,6]
> Output: 3 
> Explanation: [0,1,3,5,6] means the researcher has 5 papers in total and each of them had 
>              received 0, 1, 3, 5, 6 citations respectively. 
>              Since the researcher has 3 papers with at least 3 citations each and the remaining 
>              two with no more than 3 citations each, her h-index is 3.
> ```
>
> **Note:**
>
> If there are several possible values for _h_, the maximum one is taken as the h-index.
>
> **Follow up:**
>
> Could you solve it in logarithmic time complexity?

### 思路

1. 这个题目相当于是题目 [H-Index](/43-medium/10225-h-index.md) 的follow up，这里的输入是排好序的，然后题目里面要求 log\(N\) 的 time complexity。对于排好序的数组，很容易想到 binary search的方法来处理。
2. 转化该题目的意思，其实就是在 sorted array里面找first index，让 citations\[index\] &gt;= length\(citations\) - index，然后        \(length - index\) 就是 result的 h-index。first index保证了h-index是尽可能的最大值。因为 citations\[index\] &lt; length - index的时候，简单分析可知\(length - index\) 是没法作为 h -index 的。

### Code

```java
class Solution {
    public int hIndex(int[] citations) {
        int len = citations.length, left = 0, right = len - 1;

        while(left <= right) {
            int mid = (left + right) / 2;
            // 刚好符合定义，return h-index
            if(citations[mid] == len - mid) return len - mid;
            // 不能把 (len - mid) 作为h-index，因为不能找到(len - mid) 个paper 大于(len - mid)
            // 那么 left 右移动
            else if (citations[mid] < len - mid) left = mid + 1;
            // 此种情况是(len - mid < citations[mid])，其实(len - mid) 可能就是h-index
            // 但是可能比mid 更小的还有更大的h-index，所以还是继续寻找
            else right = mid - 1;
        }
        return len - left;
    }
}
```



