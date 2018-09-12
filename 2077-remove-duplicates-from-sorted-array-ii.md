# 80. Remove Duplicates from Sorted Array II

> Given a sorted array _nums_, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most _twice_  and return the new length.
>
> Do not allocate extra space for another array, you must do this by **modifying the input array **[**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) with O\(1\) extra memory.

### Code

```java
public class Solution {
    public int removeDuplicates(int[] A) {
        if(A.length <= 2)
           return A.length;
        int size = 1;
        int i = 1;
        // 记录上一个num的值，因为duplicate最多出现2次
        int tmp = A[0];
        while(i < A.length){
            if(A[i] != tmp){
                tmp = A[i];
                A[size++] = A[i++];
            }
            else {
                while(i < A.length && A[i] == tmp)
                      i++;
                A[size++] = tmp;
            }
        }
        return size;
    }
}
```



