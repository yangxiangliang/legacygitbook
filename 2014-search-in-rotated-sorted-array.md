### 33. Search in Rotated Sorted Array

> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
>
> \(i.e.,`0 1 2 4 5 6 7`might become`4 5 6 7 0 1 2`\).
>
> You are given a target value to search. If found in the array return its index, otherwise return -1.
>
> You may assume no duplicate exists in the array.

### 思路 1\)

在sorted array中search target，典型就会想到binary search。但是这里的array是rotated的，所以自然想到rotated的pivot那个点的左右2部分都是sorted的，所以可以先把rotated的pivot找到，然后判断target在pivot的左边还是右边，再分别对其用binary search即可。

### Code 1\)

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length == 0) return -1;
        // pivot 初始化为(nums.length-1)，因为可能长度为1的array，没有rotate的情况，那么可以认为pivot = nums.length - 1
        int pivot = nums.length-1;
        for(int i = 0; i < nums.length - 1; i ++) {
            if(nums[i] > nums[i+1]) {
                pivot = i;
                break;
            }
        }
        if(target <= nums[pivot] && target >= nums[0]) return binarySearch(nums, 0, pivot, target);
        return binarySearch(nums, pivot+1, nums.length-1, target);
    }

    private int binarySearch(int[] nums, int start, int end, int target) {
        while(start <= end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] == target) return mid;
            if(target < nums[mid]) end = mid - 1;
            else start = mid + 1;
        }
        return -1;
    }
}
```

### 思路 2）

但是上面方法complexity不是logN，因为找pivot用的for 循环，是O\(N\)。如果想总的complexity是logN，需要optimize，用binary search的方法来找pivot。然后再用binary search 分别对rotated的两边sorted的array 来查找 target。

### Code 2\)

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length == 0) return -1;
        int pivot = findPivot(nums);
        if(target <= nums[pivot] && target >= nums[0]) return binarySearch(nums, 0, pivot, target);
        return binarySearch(nums, pivot+1, nums.length-1, target);
    }

    // pivot is largest number in array
    private int findPivot(int[] nums) {
        int start = 0;
        int end = nums.length - 1;

        while(start < end) {
            int mid = (start + end) / 2;
            // 如果把rotated array看作2段sorted，这表示mid在从左到右第二段sorted array
            if(nums[mid] < nums[start]) end = mid - 1;
            else if (nums[mid] == nums[start]) {
            // 因为没有duplicate，相等情况说明 start = end - 1。但是start，end 大小无法判断
            // 取决于其是否在同一段rotated sorted array中，需要比较才得到
                return nums[end] > nums[start] ? end : start;
            // rotated的两端sorted array中，相当于mid在第一段sorted array中
            } else start = mid;
        }
        return start;
    }

    private int binarySearch(int[] nums, int start, int end, int target) {
        while(start <= end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] == target) return mid;
            if(target < nums[mid]) end = mid - 1;
            else start = mid + 1;
        }
        return -1;
    }
}
```

### 思路 3）

直接binary search，但是比普通 binary search 的模板里面做一些改动

### Code 3\)

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length == 0) return -1;
        int start = 0, end = nums.length - 1;

        // 因为每次range都至少shrink 1 size，所以这里可以取等号 "="
        while(start <= end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] == target) return mid;

            // 判断 mid, start是不是在同一段递增序列里            
            if(nums[mid] >= nums[start]) {
                if(target >= nums[start] && target < nums[mid]) end = mid - 1;
                else start = mid + 1;
            } else {
                if(target <= nums[end] && target > nums[mid]) start = mid + 1;
                else end = mid - 1;
            }
        }

        return -1;
    }
}
```



