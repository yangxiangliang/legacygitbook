# 384. Shuffle an Array

> Shuffle a set of numbers without duplicates.
>
> **Example:**
>
> ```
> // Init an array with set 1, 2, and 3.
> int[] nums = {1,2,3};
> Solution solution = new Solution(nums);
>
> // Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
> solution.shuffle();
>
> // Resets the array back to its original configuration [1,2,3].
> solution.reset();
>
> // Returns the random shuffling of array [1,2,3].
> solution.shuffle();
> ```

### Code

```java
class Solution {

    int[] nums;
    Random random;
    
    public Solution(int[] nums) {
        this.nums = nums;
        random = new Random();
    }
    
    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return nums;
    }
    
    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        int[] a = nums.clone();
        for(int i = 1; i < a.length; i ++) {
            // 把 i 和之前任何一个index swap
            int j = random.nextInt(i + 1);
            swap(a, i, j);
        }
        return a;
    }
    
    private void swap(int[] a, int i, int j) {
        int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
```



