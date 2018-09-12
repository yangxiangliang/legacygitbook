# 636. Exclusive Time of Functions

> Given the running logs of **n **functions that are executed in a nonpreemptive single threaded CPU, find the exclusive time of these functions.
>
> Each function has a unique id, start from **0 **to **n-1**. A function may be called recursively or by another function.
>
> A log is a string has this format :`function_id:start_or_end:timestamp`. For example,`"0:start:0"`means function 0 starts from the very beginning of time 0.`"0:end:0"`means function 0 ends to the very end of time 0.
>
> Exclusive time of a function is defined as the time spent within this function, the time spent by calling other functions should not be considered as this function's exclusive time. You should return the exclusive time of each function sorted by their function id.
>
> **Example 1**:
>
> ```
> Input:
> n = 2
> logs = 
> ["0:start:0",
>  "1:start:2",
>  "1:end:5",
>  "0:end:6"]
> Output:[3, 4]
> Explanation:
> Function 0 starts at time 0, then it executes 2 units of time and reaches the end of time 1. 
> Now function 0 calls function 1, function 1 starts at time 2, executes 4 units of time and end at time 5.
> Function 0 is running again at time 6, and also end at the time 6, thus executes 1 unit of time. 
> So function 0 totally execute 2 + 1 = 3 units of time, and function 1 totally execute 4 units of time.
> ```
>
> **Note:**
>
> 1. Input logs will be sorted by timestamp, NOT log id.
> 2. Your output should be sorted by function id, which means the 0th element of your output corresponds to the exclusive time of function 0.
> 3. Two functions won't start or end at the same time.
> 4. Functions could be called recursively, and will always end.
> 5. 1  &lt;= n &lt;= 100

### 思路

1. 刚开始拿到题目的时候没有什么思路，然后画图找规律去看怎么才能找到每个function的运行时间。因为每个function都保证可以end，想到了对于每个程序i，其对应的start的log相等于左括号“\(”，其对应的end的log相当于右括号"\)"。那么当碰到右括号，则需要找到前面是否有其对应的左括号即可，它们便对应了一组function的start和end。由于括号的问题常用到Stack，所以这里也想到可以用Stack来求解。
2. 这里string "0:start:0" 包含了3个信息，function的id；start or end; 运行的timing。直接将这个log string压入Stack显然不方便操作，由此想到可以另外create一个class，里面包括以上信息，一下命名这个class func，然后将每一个func压入Stack来操作即可。
3. **坑**: 这里的难点就是有些func i是在func j中call的，那么计算j的running time的时候需要减去i run掉的时候。那么需要一个东西来记录已经run掉的时间，所以还需要在create的class func中添加一个field，可以叫做"offset"，则表示在该段程序的运行过程中，被其他程序占用掉的时间。

### Code

```java
class Solution {
    class func {
        int id;
        int flag; // 0 means start, 1 means end
        int timing;
        int offset; // offset means how much time units already run for other funcs during this func running time
        public func(int id, int flag, int timing, int offset){
            this.id = id;
            this.flag = flag;
            this.timing = timing;
            this.offset = offset;
        }
    }

    public int[] exclusiveTime(int n, List<String> logs) {
        int[] rst = new int[n];
        Stack<func> stack = new Stack();
        for(String log : logs) {
            String[] tmp = log.split(":");
            int id = Integer.parseInt(tmp[0]);
            int timing = Integer.parseInt(tmp[2]);
            int flag = tmp[1].equals("start") ? 0 : 1;

            if(stack.isEmpty() || flag == 0) stack.push(new func(id, flag, timing, 0));
            else {
                func last = stack.peek();
                if(last.id == id && last.flag == 0) {
                    // 注意 end的timing是time unit的末端，start的timing是time unit的开始，所以其中包含的time units的个数
                    // 是(end-start+1)，还要减去last.offset 表示去掉该func中被其他func run掉的时间
                    rst[id] += timing - last.timing + 1 - last.offset;
                    stack.pop();
                    if(!stack.isEmpty()) {
                        // 为了update 该stack顶端func的offset值，因为其running过程中有time units已被其他func用掉
                        func update = stack.pop();
                        // 被其他func 跑掉的时间是(timing-last.timing+1-last.offset)，但是还要加上last.offset
                        // 因为last.offset也表示之前被run掉的时间，所以一共被run掉的时间(timing-last.timing+1-last.offset)+last.offset
                        // 即 (timing - last.timing + 1)
                        update.offset += timing - last.timing + 1;
                        stack.push(update);
                    }
                }
            }
        }
        return rst;
    }
}
```



