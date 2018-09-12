# 39. Combination Sum

> Given a **set **of candidate numbers \(**C**\)**\(without duplicates\) **and a target number \(**T**\), find all unique combinations in **C **where the candidate numbers sums to **T**.
>
> The **same **repeated number may be chosen from **C **unlimited number of times.
>
> **Note:**
>
> * All numbers \(including target\) will be positive integers.
> * The solution set must not contain duplicate combinations.
>
> For example, given candidate set`[2, 3, 6, 7]`and target`7`,  
> A solution set is:
>
> ```
> [
>   [7],
>   [2, 2, 3]
> ]
> ```

### 思路

1. 因为candidates中任何个元素都可能在最后结果的组合中，所以只有去尝试每个组合的情况，如果可以得到sum等于target，则加入结果中，反之不加入结果中。典型的backtracking的思想。
2. 这里因为最后结果不能contain duplicate combinations，想办法avoid duplication，而且一个candidate可以用多次。比如题目例子中，选了\[2, 2, 3\]是一种valid combination，下次如果先选3，那么不能“倒”回去选2，不能出现\[3, 2, 2\]的情况。所以想到在backtracking的recursion子函数中需要parse一个index，表示current candidate的index，那么之后recursion的函数选candidate的时候，只能从该index以及之后的元素中选择，不能从candidates的一开始从头选择，这样avoid duplicate combinations。

### 复杂度

Time: O\(2^N\), N是candidates的长度。这个是backtracking，看起解空间树的情况，相当于每个candidates都有可能有2种状态，一种是选入最后结果的set，一种是没有选入最后的set，一共有N个candidates，解空间的complexity是O\(2^N\)

### Code

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> rst = new ArrayList();
        sum(candidates, 0, target, rst, new ArrayList());
        return rst;
    }

    // index 表示选择到current candidate在整个candidates中的index
    private void sum(int[] candidates, int index, int target, List<List<Integer>> rst, List<Integer> list) {
        if(target == 0) {
            rst.add(new ArrayList(list));
            return;
        }

        for(int i = index; i < candidates.length; i ++) {
            // 因为candidates都是positive的，所以如果candidate比target大，那么肯定不能放入结果中
            if(candidates[i] > target) continue;
            list.add(candidates[i]);
            sum(candidates, i, target-candidates[i], rst, list);
            list.remove(list.size()-1);
        }
    }
}
```



