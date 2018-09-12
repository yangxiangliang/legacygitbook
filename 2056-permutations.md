# 46.  Permutations

> Given a collection of **distinct **integers, return all possible permutations.
>
> **Example:**
>
> ```
> Input: [1,2,3]
> Output:
> [
>   [1,2,3],
>   [1,3,2],
>   [2,1,3],
>   [2,3,1],
>   [3,1,2],
>   [3,2,1]
> ]
> ```

### 思路

1. 典型的 backtracking 思想。因为每个元素都需要遍历，但是前后遍历顺序可以不同。可以用一个boolean\[\] 记录当前遍历过程中已经遍历了哪些元素，只把没有visit的元素加入current path 当中。
2. 注意backtracking的时候，current path 走完之后，需要remove掉最后加入的元素，才返回上一层的 backtracking 过程。即“剪枝”

### Code

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> rst = new ArrayList();
        boolean[] visited = new boolean[nums.length];
        helper(nums, visited, rst, new ArrayList());
        return rst;
    }

    private void helper(int[] nums, boolean[] visited, List<List<Integer>> rst, List<Integer> path) {
        if(path.size() == nums.length) {
            rst.add(new ArrayList(path));
            return;
        }

        for(int i = 0; i < nums.length; i ++) {
            if(!visited[i]) {
                path.add(nums[i]);
                visited[i] = true;
                helper(nums, visited, rst, path);
                // remove 掉当前元素，把当前元素visiit的状态set为false。即相当于没有visit当前元素，再返回到上一层backtracking
                visited[i] = false;
                path.remove(path.size() - 1);
            }
        }
    }
}
```



