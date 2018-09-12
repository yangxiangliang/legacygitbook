# 218. The Skyline Problem

> A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are **given the locations and height of all the buildings **as shown on a cityscape photo \(Figure A\), write a program to **output the skyline **formed by these buildings collectively \(Figure B\).
>
> ![](/assets/skyline1.jpg)
>
> ![](/assets/skyline2.jpg)
>
> The geometric information of each building is represented by a triplet of integers`[Li, Ri, Hi]`, where`Li`and`Ri`are the x coordinates of the left and right edge of the ith building, respectively, and`Hi`is its height. It is guaranteed that`0 < Li < Ri < INT_MAX`,`0 < Hi < INT_MAX`, and`Ri - Li > 0`. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.
>
> For instance, the dimensions of all buildings in Figure A are recorded as:`[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ]`.
>
> The output is a list of "**key points**" \(red dots in Figure B\) in the format of `[ [x1,y1], [x2, y2], [x3, y3], ... ]`that uniquely defines a skyline. **A key point is the left endpoint of a horizontal line segment**. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.
>
> For instance, the skyline in Figure B should be represented as:
>
> `[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0 ]`.
>
> **Note: **
>
> * The number of buildings in any input list is guaranteed to be in the range \[0, 10000\].
>
> * The input list is already sorted in ascending order by the left x position Li.
>
> * The output list must be sorted by the x position.
>
> * There must be no consecutive horizontal lines of equal height in the output skyline. For instance, \[...\[2 3\], \[4 5\], \[7 5\], \[11 5\], \[12 7\]...\]  is not acceptable; the three lines of height 5 should be merged into one in the final output as such: \[...\[2 3\], \[4 5\], \[12 7\], ...\]

## 思路

首先想怎么样表示一个矩形，我们可以用每个矩形的上面的的左，右顶点来表示这个矩形。所以我们一开始先从input里取出左边和右边顶点，放入一个list，并且sort该list，让这些点按照x坐标从左往右排列。注意，这里为了区分一条边的左顶点和右顶点，我们把左顶点的height表示为negative的，右边顶点的height仍为positive的。观察图B可以看出，相邻的2个key point的高度都不一样，即遍历之前的矩形的边界points的时候，如果point的高度发生变化时候，我们要考虑把其放入结果的key points中。因为我们只记录skyline的points，skyline只会在乎在x坐标相同的情况下，y坐标\(height\)更高的点，所以考虑用max heap来放入矩形的每个顶点。如果遇到矩形的左顶点，那么放入heap，如果遇到右顶点，从heap中remove掉，表示该height的边已经结束了。同时需要记录之前的max height，和当前的max height，如果在heap操作后，current的max height和previous max height不同了，则需要将其放入结果的key points中。

### 注意

因为地平线的height为0，初始化max heap时，需要把 height = 0 放入heap，表示地平线的高度。

### 复杂度

Time O\(n \* log\(n\)\), Space O\(n\)

### Code

```java
public class Solution {
    public List<int[]> getSkyline(int[][] buildings) {
        List<int[]> result = new ArrayList();
        List<int[]> points = new ArrayList(); // 记录矩形的各个顶点
        for(int[] building : buildings) {
            points.add(new int[]{building[0], -building[2]}); // 左顶点的height为negative，与右顶点加以区分
            points.add(new int[]{building[1], building[2]});
        }
        
        Collections.sort(points, new Comparator<int[]>() {
           public int compare(int[] a, int[] b) {
               // 按照x坐标从左到右排序，如果相等，按照height从低到高排序
               return a[0] == b[0] ? a[1] - b[1] : a[0] - b[0];
           } 
        });
        
        Queue<Integer> maxHeap = new PriorityQueue<Integer>(10000, new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                return b - a;
            }
        });
        
        int pre = 0;
        maxHeap.offer(0); // 加入初始化的高度，即地平线的高度 0
        for(int[] point : points) {
            if(point[1] < 0) maxHeap.offer(-point[1]);
            else maxHeap.remove(point[1]);
            
            int cur = maxHeap.peek();
            if(pre != cur) {
                result.add(new int[]{point[0], cur});
                pre = cur;
            }
        }
        return result;
    }
}
```



