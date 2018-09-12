# 379. Design Phone Directory

> Design a Phone Directory which supports the following operations:
>
> 1. **get**: Provide a number which is not assigned to anyone.
> 2. **check**: Check if a number is available or not.
> 3. **release**: Recycle or release a number.
>
> **Example**:
>
> ```
> // Init a phone directory containing a total of 3 numbers: 0, 1, and 2.
> PhoneDirectory directory = new PhoneDirectory(3);
>
> // It can return any available phone number. Here we assume it returns 0.
> directory.get();
>
> // Assume it returns 1.
> directory.get();
>
> // The number 2 is available, so return true.
> directory.check(2);
>
> // It returns 2, the only number that is left.
> directory.get();
>
> // The number 2 is no longer available, so return false.
> directory.check(2);
>
> // Release number 2 back to the pool.
> directory.release(2);
>
> // Number 2 is available again, return true.
> directory.check(2);
> ```

### 思路

* 用1个HashSet记录使用过的number，release的时候，从set中remove掉
* 用1个queue记录所有available的number，release的number也加入queue中

### Code

```java
public class PhoneDirectory {
    HashSet<Integer> used = new HashSet();
    Queue<Integer> queue = new LinkedList();

    /** Initialize your data structure here
        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
    public PhoneDirectory(int maxNumbers) {
        for (int i=0; i<maxNumbers; i++) {
            queue.offer(i);
        }
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    public int get() {
        if (queue.size() > 0) {
            int number = queue.poll();
            used.add(number);
            return number;
        }
        return -1;
    }
    
    /** Check if a number is available or not. */
    public boolean check(int number) {
        return !used.contains(number);
    }
    
    /** Recycle or release a number. */
    public void release(int number) {
        if (used.contains(number)) {
            used.remove(number);
            queue.offer(number);
        }
    }
}

/**
 * Your PhoneDirectory object will be instantiated and called as such:
 * PhoneDirectory obj = new PhoneDirectory(maxNumbers);
 * int param_1 = obj.get();
 * boolean param_2 = obj.check(number);
 * obj.release(number);
 */
```



