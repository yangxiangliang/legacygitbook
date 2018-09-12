# 735. Asteroid Collision

> We are given an array`asteroids`of integers representing asteroids in a row.
>
> For each asteroid, the absolute value represents its size, and the sign represents its direction \(positive meaning right, negative meaning left\). Each asteroid moves at the same speed.
>
> Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.
>
> **Example 1:**
>
> ```
> Input: 
> asteroids = [5, 10, -5]
> Output: [5, 10]
> Explanation: 
> The 10 and -5 collide resulting in 10.  The 5 and 10 never collide.
> ```
>
> **Example 2:**
>
> ```
> Input: 
> asteroids = [8, -8]
> Output: []
> Explanation: 
> The 8 and -8 collide exploding each other.
> ```
>
> **Example 3:**
>
> ```
> Input: 
> asteroids = [10, 2, -5]
> Output: [10]
> Explanation: 
> The 2 and -5 collide resulting in -5.  The 10 and -5 collide resulting in 10.
> ```
>
> **Example 4:**
>
> ```
> Input: 
> asteroids = [-2, -1, 1, 2]
> Output: [-2, -1, 1, 2]
> Explanation: 
> The -2 and -1 are moving left, while the 1 and 2 are moving right.
> Asteroids moving the same direction never meet, so no asteroids will meet each other.
> ```
>
> **Note：**
>
> * The length of `asteroids`will be at most`10000`.
>
> * Each asteroid will be a non-zero integer in the range`[-1000, 1000].`

### 思路

1. 一开始拿到问题的时候，先看哪种石头会meet。因为同一个方向的不会meet，所以同一种符号的石头不会meet。不同符号的情况下，左边负数，右边正数也不会meet，因为左边继续往左，右边继续往右。只有左边正数，右边负数的时候会meet，meet后还要根据石头的大小来判断“保留”哪个石头。
2. 然后想先用一个什么数据结构来保留剩下的石头，比如i和\(i+1\) meet了，那么要拿出i，然后把i和\(i+1\) 保留下的那个石头放入数据结构，此时还要检测该石头和\(i-1\)是否meet，可能又有石头explode。这里感觉“拿出最上面的石头，相比较，然后explode一个，再与倒数第二上面的石头比较，看是否meet.....”。显然用stack 解决最符合这种感觉

### Code

```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        Stack<Integer> stack = new Stack();
        for(int i = 0; i < asteroids.length; i++) {
            int cur = asteroids[i];
            if(!stack.isEmpty() && cur < 0 && stack.peek() > 0)  {
                if( (-cur) >= stack.peek()) {
                    // 这时候stack最上面的肯定explode，如果(-cur)更大，那么需要cur与stack倒数第二上面的比较
                    // 看是否会meet，所以i--，与之后i++抵消，相当于让i石头再循环一次，和之后stack的顶端比较
                    if((-cur) > stack.peek()) i--;
                    stack.pop();
                }
            } else stack.push(cur);
        }
        int[] rst = new int[stack.size()];
        for(int i = rst.length - 1; i >= 0; i --) rst[i] = stack.pop();
        return rst;
    }
}
```



