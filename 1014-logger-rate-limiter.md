# 359. Logger Rate Limiter

> Design a logger system that receive stream of messages along with its timestamps, each message should be printed if and only if it is **not printed in the last 10 seconds**.
>
> Given a message and a timestamp \(in seconds granularity\), return true if the message should be printed in the given timestamp, otherwise returns false.
>
> It is possible that several messages arrive roughly at the same time.
>
> **Example: **
>
> ```
> Logger logger = new Logger();
> // logging string "foo" at timestamp 1
> logger.shouldPrintMessage(1, "foo"); returns true; 
>
> // logging string "bar" at timestamp 2
> logger.shouldPrintMessage(2,"bar"); returns true;
>
> // logging string "foo" at timestamp 3
> logger.shouldPrintMessage(3,"foo"); returns false;
>
> // logging string "bar" at timestamp 8
> logger.shouldPrintMessage(8,"bar"); returns false;
>
> // logging string "foo" at timestamp 10
> logger.shouldPrintMessage(10,"foo"); returns false;
>
> // logging string "foo" at timestamp 11
> logger.shouldPrintMessage(11,"foo"); returns true;
> ```

### 思路

用1个hashMap记录message以及其最近一次被print 出来的时间。如果有相同的message进来，需要比较current timestamp与上一次print出来的时间是否&gt;=10，if true，则print出来，并且update map里的该message的时间；if false，则不print，**并且不能update map里message对应的时间，因为这次根本没有print该message**。

### Code

```java
public class Logger {
    HashMap<String, Integer> map;
    
    /** Initialize your data structure here. */
    public Logger() {
        map = new HashMap();
    }
    
    /** Returns true if the message should be printed in the given timestamp, otherwise returns false.
        If this method returns false, the message will not be printed.
        The timestamp is in seconds granularity. */
    public boolean shouldPrintMessage(int timestamp, String message) {
        if(!map.containsKey(message)) {
            map.put(message, timestamp);
            return true;
        }
        int lastTime = map.get(message);
        if(timestamp - lastTime >= 10) {
            map.put(message, timestamp);
            return true;
        }
        return false;
    }
}

/**
 * Your Logger object will be instantiated and called as such:
 * Logger obj = new Logger();
 * boolean param_1 = obj.shouldPrintMessage(timestamp,message);
 */
```



