# 406. Queue Reconstruction by Height

> Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers `(h, k)`, where `h`is the height of the person and `k`is the number of people in front of this person who have a height greater than or equal to `h`. Write an algorithm to reconstruct the queue.
>
> **Note: **
>
> The number of people is less than 1,100.
>
> **Example:**
>
> ```
> Input:
> [[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]
>
> Output:
> [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
> ```

### 思路

因为k表示之前有多少个height大于等于该people的人数，想到可以按照height从高到低排序，如果height相同，那么按照k的从小到大排序，因为k较大的应该更应该排在后面。此时用list&lt;int\[\]&gt; rst表示结果，遍历sorted input people，如果 index值和该people的k值不相同，那么表示该people应该前移动，则应该 rst.add\(k of current people, current people\)，即把current people insert到rst的index = k值处。如果 index值和该people的K值相同，那么表示其在合适的位置，直接add到rst的队尾即可。

### 注意

其实遍历sorted input people时候，index是否和current people的K值相等，这2种情况可以合并为一种情况。如果index和current people的K值相等，说明current people K值也就是rst队尾的index的值。

### Code

```java
public class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                if(a[0] != b[0]) return b[0] - a[0];
                else return a[1] - b[1];
            }
        });

        List<int[]> rst = new ArrayList();
        for(int i = 0; i < people.length; i ++) {
            // 也可以写为：
            // if(people[i][1] != i) rst.add(people[i][1], people[i]);
            // else rst.add(people[i]);
            rst.add(people[i][1], people[i]);
        }
        // convert list to array
        return rst.toArray(new int[rst.size()][]);
    }
}
```



