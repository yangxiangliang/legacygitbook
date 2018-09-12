# 716. Max Stack

> Design a max stack that supports push, pop, top, peekMax and popMax.
>
> 1. push\(x\) -- Push element x onto stack.
> 2. pop\(\) -- Remove the element on top of the stack and return it.
> 3. top\(\) -- Get the element on the top.
> 4. peekMax\(\) -- Retrieve the maximum element in the stack.
> 5. popMax\(\) -- Retrieve the maximum element in the stack, and remove it. If you find more than one maximum elements, only remove the top-most one.
>
> **Example 1:**
>
> ```
> MaxStack stack = new MaxStack();
> stack.push(5); 
> stack.push(1);
> stack.push(5);
> stack.top(); -> 5
> stack.popMax(); -> 5
> stack.top(); -> 1
> stack.peekMax(); -> 5
> stack.pop(); -> 1
> stack.top(); -> 5
> ```
>
> **Note:**
>
> 1. -1e7 &lt;= x &lt;= 1e7
> 2. Number of operations won't exceed 10000.
> 3. The last four operations won't be called when stack is empty.

### 思路

1. 和经典的 "Min Stack" 想法一样，就是用2个stack。一个regular的stack，一个max stack存当前regular stack中的max value。
2. 注意这里因为需要支持 popMax\(\)操作, 比如往stack 中push入5，1；max stack中存5。然后如果popMax\(\)时，应该pop出5，此时pop出来regular stack元素 的时候，也同时可能 pop 处max stack中对应的元素，把regular stack中的元素存在一个临时的stack中。然后再把临时的temp stack中的元素push入原来是的regular stack和max stack中。

### Code

```java
class MaxStack {
    Stack<Integer> stack;
    Stack<Integer> max;

    /** initialize your data structure here. */
    public MaxStack() {
        stack = new Stack();
        max = new Stack();
    }

    public void push(int x) {
        stack.push(x);
        if(max.isEmpty() || x >= max.peek()) max.push(x);
    }

    public int pop() {
        int rst = stack.pop();
        if(rst == max.peek()) max.pop();
        return rst;
    }

    public int top() {
        return stack.peek();
    }

    public int peekMax() {
        return max.peek();
    }

    public int popMax() {
        // temp stack 用来buffer 暂时pop出来的元素
        Stack<Integer> tmp = new Stack();
        // pop普通stack的时候，也同时把max stack中对应max value pop出来，所以用该class customized的pop 函数
        while(stack.peek() < max.peek()) tmp.push(pop());
        int rst = pop();
        // 在push 入 temp stack中的元素的时候，也用class customized的push 函数
        // 因为边往stack里面添加元素的时候，也要往max stack里面添加元素
        while(!tmp.isEmpty()) push(tmp.pop());
        return rst;
    }
}

/**
 * Your MaxStack object will be instantiated and called as such:
 * MaxStack obj = new MaxStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.peekMax();
 * int param_5 = obj.popMax();
 */
```



