# 78. Subsets

> Given a set of **distinct **integers, nums, return all possible subsets \(the power set\).
>
> **Note: **The solution set must not contain duplicate subsets.
>
> For example,  
> If **nums **=`[1,2,3]`, a solution is:
>
> ```
> [
>   [3],
>   [1],
>   [2],
>   [1,2,3],
>   [1,3],
>   [2,3],
>   [1,2],
>   []
> ]
> ```

### 思路

遍历nums中的所有情况，对应每个integer只有2种可能性，即加入最后的subset 和 不加入最后的subset。用List&lt;Integer&gt; 记录该path中被加入的integer，就是最后的subset。典型的Backtracking感觉

### 复杂度

Time: O\(2^N\), N是nums数组的长度。还是来看解空间，每个integer对应2种情况，可能在subset中，可能不在。所以解空间的复杂度是O\(2^N\)

### Code

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> rst = new ArrayList();
        build(nums, 0, rst, new ArrayList());
        return rst;
    }
    
    private void build(int[] nums, int index, List<List<Integer>> rst, List<Integer> list) {
        if(index == nums.length) {
            rst.add(new ArrayList(list));
            return;
        }
        // add nums[index]
        list.add(nums[index]);
        build(nums, index+1, rst, list);
        list.remove(list.size() - 1);
        
        // do not add nums[index]
        build(nums, index+1, rst, list);
    }
}
```



