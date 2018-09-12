# 90. Subsets II

> Given a collection of integers that might contain duplicates, _**nums**_, return all possible subsets \(the power set\).
>
> **Note: **The solution set must not contain duplicate subsets.
>
> **Example:**
>
> ```
> Input: [1,2,2]
> Output:
> [
>   [2],
>   [1],
>   [1,2,2],
>   [2,2],
>   [1,2],
>   []
> ]
> ```

### 思路

这里写法和 [subsets](/subset) 题目有一些不一样，因为有duplicate，而且最后结果不能有duplicate subsets。一开始把array sort一下， 那么duplicate的元素都挨在一起了，便于查找duplicate的元素。

不同的地方就是：\*\*如果nums\[i\], nums\[i+1\]的值相等，那么取nums\[i+1\]放入结果的时候，只能是在nums\[i\]已经放入结果path中才行。如果nums\[i\]不在结果的path中，那么不能把nums\[i+1\]放入结果中，不然会有duplicates。

### Code

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> rst = new ArrayList();
        Arrays.sort(nums);
        dfs(nums, 0, new ArrayList(), rst);
        return rst;
    }

    private void dfs(int[] nums, int start, List<Integer> path, List<List<Integer>> rst) {
        if(start == nums.length) {
            rst.add(new ArrayList(path));
            return;
        }
        
        // nums[start] 加入结果中
        path.add(nums[start]);
        dfs(nums, start + 1, path, rst);
        path.remove(path.size() - 1);

        // 表示如果nums[start]和nums[start+1]相同，那么在nums[start]不加入结果的时候，nums[start+1]也不能加入结果中
        while(start < nums.length - 1 && nums[start] == nums[start + 1]) start ++;
        // 不把 nums[start] 加入结果中
        dfs(nums, start + 1, path, rst);
    }
}
```



