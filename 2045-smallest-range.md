# 632. Smallest Range

> You have`k`lists of sorted integers in ascending order. Find the **smallest **range that includes at least one number from each of the`k`lists.
>
> We define the range \[a,b\] is smaller than range \[c,d\] if `b-a < d-c`or`a < c`if`b-a == d-c`.
>
> **Example 1:**
>
> ```
> Input:[[4,10,15,24,26], [0,9,12,20], [5,18,22,30]]
> Output: [20,24]
> Explanation: 
> List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
> List 2: [0, 9, 12, 20], 20 is in range [20,24].
> List 3: [5, 18, 22, 30], 22 is in range [20,24].
> ```
>
> **Note:**
>
> 1. The given list may contain duplicates, so ascending order means &gt;= here.
> 2. 1 &lt;= k &lt;= 3500
> 3. -10^5 &lt;= value of elements &lt;= 10^5

### 思路

以这个题目为例子，来找最小的range至少都包含3个list中的一个元素。

4，10，15，24，26

0，9，12，20

5，18，22，30

一开始取3个list中的最小元素，\(4, 0, 5\)，看这个range的大小。然后想办法怎么才能变化把这个range缩小，确依然能包含3个list中的元素。因为每个list的increasing的，如果移动4或则5，在其list往前走，那么这个range肯定是变得更大的。因为看\(4, 0, 5\)中的最小的元素，因为相当于该元素托在后面，“拖了后腿”，把该元素在其list中往前移动一位。看\(4, 9, 5\) 新的range \[4, 9\] 的大小和前面range相互比较即可。

1. 所以这里需要记录每次range中最小值所在的list的index，然后把其往后面移动一位，形成新的range。因为记录“最小值”，所以想到用min heap即可。但是需要记录该element在输入List&lt;List&lt;Integer&gt;&gt; nums 中是第几个list里面，然后在该list是第几个。所以可以create另外的class来存这个信息。
2. 当然每次移动后，还要记录当前遍历到的最大值，便是当前range的最大值，用来计算range的长度。

### Code

```java
class Solution {
    public int[] smallestRange(List<List<Integer>> nums) {
        PriorityQueue<Element> minHeap = new PriorityQueue<Element>(new Comparator<Element>(){
            public int compare(Element a, Element b) {
                return nums.get(a.row).get(a.col) - nums.get(b.row).get(b.col);
            }
        });

        int max = Integer.MIN_VALUE;
        for(int i = 0; i < nums.size(); i ++) {
            minHeap.offer(new Element(i, 0));
            max = Math.max(max, nums.get(i).get(0));
        }

        int[] rst = {Integer.MIN_VALUE, Integer.MAX_VALUE};
        while(!minHeap.isEmpty()) {
            Element min = minHeap.poll();

            int lowerBound = nums.get(min.row).get(min.col);
            
            // 当 max - lowerBound == rst[1] - rst[0] 的时候，因为题目里面说取 rst[0] 较小的情况
            // 所以就 keep 原来的 rst 里面的值即可
            if(rst[0] == Integer.MIN_VALUE || max - lowerBound < rst[1] - rst[0]) {
                rst = new int[]{lowerBound, max};
            }

            // 表示min.row 对应 list 已经遍历到最后的元素了，不能再往下遍历新元素了
            if(min.col == nums.get(min.row).size() - 1) break;
            minHeap.offer(new Element(min.row, min.col + 1));
            max = Math.max(max, nums.get(min.row).get(min.col + 1));
        }
        return rst;
    }

    class Element{
        int row; // index of which list contains this element is in original nums
        int col; // index of element in its list

        public Element(int row, int col) {
            this.row = row;
            this.col = col;
        }
    }
}
```



