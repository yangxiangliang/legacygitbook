# 75. Sort Colors

> Given an array with _n_ objects colored red, white or blue, sort them [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) so that objects of the same color are adjacent, with the colors in the order red, white and blue.
>
> Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.  
> **Note: **You are not supposed to use the library's sort function for this problem.
>
> **Example:**
>
> ```
> Input: [2,0,2,1,1,0]
>
> Output: [0,0,1,1,2,2]
> ```
>
> **Follow up: **Could you come up with a one-pass algorithm using only constant space?

### 思路

1. 因为这里需要 one-pass，而且只能用constant space，in-place。所以想到只能在数组内部swap。用一个start pointer 指向头部，如果遇见0，则和start处swap，然后start ++；同理需要一个end pointer 指向尾部，如果遇见2，则和end处swap，然后end--

### Code

```java
class Solution {
    public void sortColors(int[] nums) {
        int start = 0;
        int end = nums.length - 1;
        for(int i = 0; i < nums.length;) {
            if(i > start && nums[i] == 0) {
                swap(nums, i, start);
                start ++;
            } else if(i < end && nums[i] == 2) {
                swap(nums, i, end);
                end --;
            } else i ++;
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```



