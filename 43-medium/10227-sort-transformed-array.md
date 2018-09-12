# 360. Sort Transformed Array

> Given a **sorted **array of integers _nums_ and integer values _a_, _b_ and _c_. Apply a function of the form f\(x\) = ax^2+bx+c to each element _x_ in the array.
>
> The returned array must be in **sorted order**.
>
> Expected time complexity: **O\(n\)**

### 思路 1\)

这道题目有关抛物线的知识，如果 a = 0，那么transformed后的数组也是sorted，那么sorted的顺序是否和之前一样，是由b的正负值所决定的。如果 a != 0，那么transformed后的值是和距离抛物线中间 - b / 2a的距离相关的，如果 a &gt; 0，距离中间越远的点transformed后的值越大；如果 a &lt; 0，距离中间越远的点transformed后的值越小。因为这里input是sorted的，所以可以一开始用binary search找出抛物线的中点附近的index，然后从该index开始，分别开始往左和右遍历array，类似于merge sort的思路，加入最后的结果中。 

### Code 1\)

```java
public class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int[] rst = new int[nums.length];
        double middle = (double) nums[0];
        if(a != 0) middle = - (double) b / (double) (a * 2);
        int index = binarySearch(nums, 0, nums.length - 1, middle);
        int left = index - 1;
        int right = index;
        // reverse = true 说明transformed后的sorted order和input的order不同
        // 此时则可以从rst的右边到左边1个1个放入transformed的值
        boolean reverse = false;
        if(a < 0 || (a == 0 && b < 0)) reverse = true;
        int i = reverse ? nums.length - 1 : 0;
        
        while(left >=0 || right < nums.length) {
            if(left < 0) rst[i] = calculate(nums[right++], a, b, c);
            else if(right >= nums.length) rst[i] = calculate(nums[left--], a, b, c);
            else if (middle - nums[left] > nums[right] - middle) {
                rst[i] = calculate(nums[right++], a, b, c);
            } else rst[i] = calculate(nums[left--], a, b, c);
            i = reverse ? i -1 : i + 1;
        }
        return rst;
    }
    
    public int binarySearch(int[] nums, int start, int end, double target) {
        while(start < end) {
            int mid = start + (end - start) / 2;
            if((double) nums[mid] < target) start = mid + 1;
            else end = mid;
        }
        return end;
    }
    
    public int calculate(int x, int a, int b,int c) {
        return a*x*x + b*x + c;
    }
}
```

### 思路 2\)

思路和 **\(1\)** 中类似，用merge sort的思想。但是不用binary search开始从中间开始找，从两边开始往中间寻找，i = 0， j = nums.length - 1 ，如果 a &gt; 0，取transformed\(i\)和transformed\(j\)中的较大者，从结果数组的右边向左边开始排列。如果 a &lt; 0，取transformed\(i\)和transformed\(j\)中的较小者，在结果数组中从左到右排列。a = 0的情况，可以归入上面2种任一情况即可。

### Code 2\)

```java
public class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int[] rst = new int[nums.length];
        int i = 0, j = nums.length - 1;
        int index = a >= 0 ? nums.length - 1 : 0;
        
        while(i <= j) {
            if(a >= 0) {
                rst[index--] = calculate(nums[i], a, b, c) <= calculate(nums[j], a, b, c) ? calculate(nums[j--], a, b, c) : calculate(nums[i++], a, b, c);
            } else rst[index++] = calculate(nums[i], a, b, c) <= calculate(nums[j], a, b, c) ? calculate(nums[i++], a, b, c) : calculate(nums[j--], a, b, c);
        }
        return rst;
    }
    
    public int calculate(int x, int a, int b,int c) {
        return a*x*x + b*x + c;
    }
}
```



