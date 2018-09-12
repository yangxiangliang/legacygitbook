# 155. Min Stack

> Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
>
> * push\(x\) -- Push element x onto stack.
> * pop\(\) -- Removes the element on top of the stack.
> * top\(\) -- Get the top element.
> * getMin\(\) -- Retrieve the minimum element in the stack.

1\) 用两个stack, 一个记录Min值，一个regular stack记录所有的值

```java
class MinStack {
    Stack<Integer> stack = new Stack();
    Stack<Integer> min = new Stack();
    public void push(int x) {
        stack.push(x);
        if(min.isEmpty() || x <= min.peek())
          min.push(x);
    }

    public void pop() {
      int tmp = stack.pop();
      if(tmp <= min.peek())
         min.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return min.peek();
    }
}

```

2\) 只用1个Stack, 然后用一个min记录目前stack里的最小值。push的时候，如果x &lt;= min, 则先将min push into stack, min是除x之外其他的最小值，然后reset min为x。如果pop出的值是min 值，则多pop一个元素，这个值便是剩下元素的最小值。

```java
public class MinStack {
    int min = Integer.MAX_VALUE;
    Stack<Integer> stack = new Stack();

    /** initialize your data structure here. */
    public MinStack() {}
    
    public void push(int x) {
        // 如果要改变最小值min时，则先把原来的min push into stack，记录此时stack中元素的最小值。
        if (x <= min) {
            stack.push(min);
            min = x;
        }
        stack.push(x);
    }
    
    public void pop() {
        // 如果pop出的值是最小值，再pop一次得到剩下元素的最小值
        if (min == stack.pop()) min = stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return min;
    }
}
```



