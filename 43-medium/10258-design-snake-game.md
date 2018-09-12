# 353. Design Snake Game

> Design a [Snake game](https://en.wikipedia.org/wiki/Snake_%28video_game%29) that is played on a device with screen size = width x height. [Play the game online](http://patorjk.com/games/snake/) if you are not familiar with the game.
>
> The snake is initially positioned at the top left corner \(0,0\) with length = 1 unit.
>
> You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.
>
> Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.
>
> When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.
>
> **Example:**
>
> ```
> Given width = 3, height = 2, and food = [[1,2],[0,1]].
>
> Snake snake = new Snake(width, height, food);
>
> Initially the snake appears at position (0,0) and the food at (1,2).
>
> |S| | |
> | | |F|
>
> snake.move("R"); -> Returns 0
>
> | |S| |
> | | |F|
>
> snake.move("D"); -> Returns 0
>
> | | | |
> | |S|F|
>
> snake.move("R"); -> Returns 1 (Snake eats the first food and right after that, the second food appears at (0,1) )
>
> | |F| |
> | |S|S|
>
> snake.move("U"); -> Returns 1
>
> | |F|S|
> | | |S|
>
> snake.move("L"); -> Returns 2 (Snake eats the second food)
>
> | |S|S|
> | | |S|
>
> snake.move("U"); -> Returns -1 (Game over because snake collides with border)
> ```

### 思路

这道题主要的问题就是记录该snake的body所占的unit的位置和snake的头的位置，因为每次move后eat food是取决于snake的head的位置。

* 首先用foodIndex记录当前应该吃food中的哪一个food，然后x, y记录的是当前snake的head的位置，length记录的是snake的长度。只要snake没有超出边界和bite 自己，那么move后return的就是 snake的长度 减 1
* 关键是怎么记录snake的body，因为需要判断其是否可能在move后bite到自己。这里每次move后，如果没有吃到food，snake的尾巴的unit就需要remove掉，然后head变为新到的move到的点。如果吃到food，因为长度需要加1，所以snake的尾巴不用remove掉，直接把head变为新到的move到的点。**这种操作类似于Deque**， 因为Deque 既可以从tail remove掉元素，也可以从 head处加入新元素，所以这里用Deque来记录snake的body。
* 如果只有上面提到的Deque，那么判断snake是否bite自己的时候，需要查看Deque中所有点，新的head如果和其中某个点重合，说明bite了自己，这样很不efficient。想到用一个HashSet来记录snake所有body点，方便查找。显然HashSet中不点int\[2\]，为了方便，可以用一个Integer来表示点int\[2\] point，常用的方法就是 hash\(int\[2\] point\) = width \* point\[0\] + point\[1\]，相当于把2D的matrix按照row拉开排成一列，每个point都有对应的index，这样int\[2\] point都有唯一对应的Integer来表示其位置。

### Code

```java
class SnakeGame {
    int foodIndex, width, height, length, x, y;
    int[][] food;
    // 记录body中所有unit的位置，方便查找
    HashSet<Integer> body = new HashSet();
    // 记录body中所有unit，方便move的时候remove tail and add new head
    Deque<Integer> snake = new LinkedList();


    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    public SnakeGame(int width, int height, int[][] food) {
        foodIndex = 0;
        this.width = width;
        this.height = height;
        length = 1;
        x = 0;
        y = 0;
        this.food = food;
        body.add(0); // (0, 0)为起始的head
        snake.addFirst(0); // 加入起始的head (0, 0)
    }

    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body. */
    public int move(String direction) {
        switch(direction) {
            case "U": x --;
                break;
            case "L": y --;
                break;
            case "R": y ++;
                break;
            default: x ++;
        }
        // cross borders
        if(x < 0 || x >= height || y < 0 || y >= width) return -1;
        
        // 根据位置的x, y坐标得到hash 后的代表position的Integer值
        int pos = hashBody(x, y);
        // 说明吃到了food
        if(foodIndex < food.length && x == food[foodIndex][0] && y == food[foodIndex][1]) {
            length ++;
            foodIndex ++;
        } else {
            // 没吃到food, 需要remove掉tail unit 
            body.remove(snake.pollLast());
            // new head如果和body的unit重复，说明bite到自己
            if(body.contains(pos)) return -1;
        }
        
        // 加入新的head
        snake.addFirst(pos);
        body.add(pos);
        return length - 1;
    }

    public int hashBody(int i, int j) {
        return width * i + j;
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame obj = new SnakeGame(width, height, food);
 * int param_1 = obj.move(direction);
 */
```



