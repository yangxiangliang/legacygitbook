# 42. Trapping Rain Water

> Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.
>
> **For example,**
>
> Given `[0,1,0,2,1,0,1,3,2,1,2,1]`, return`6`.
>
> ![](/assets/rainwatertrap.png)
>
> The above elevation map is represented by array \[0,1,0,2,1,0,1,3,2,1,2,1\]. In this case, 6 units of rain water \(blue section\) are being trapped.

### 思路

首先需要弄清楚，对于某一个x轴上面的坐标处，该处能装的水量是由是决定的。其实也和该点左边的最高点，和右边的最高点相关的。假设左边最高点的值是left，右边的最高点的值是right，是由这2个值中的较小的值决定的，即"木桶原理"。所以可以用1个数组left\[i\]表示i点左边的最高点，另外1个数组right\[i\] 表示i点右边的最高点。那么取 int bound = min\(left\[i\], right\[i\]\)，如果bound大于height\[i\]的值，那么i的存水量就等于 \(bound - i的高度\)，即 bound - height\[i\]。

### Code

```java
public class Solution {
    public int trap(int[] height) {
        int[] left = new int[height.length];
        int[] right = new int[height.length];
        for(int i = 1; i < height.length; i ++) {
            left[i] = Math.max(left[i-1], height[i-1]);
            right[height.length - 1 - i] = Math.max(right[height.length - i], height[height.length - i]);
        }

        int rst = 0;
        for(int i = 1; i < height.length - 1; i ++) {
            int bound = Math.min(left[i], right[i]);
            if(bound > height[i]) rst += bound - height[i];
        }
        return rst;
    }
}
```



