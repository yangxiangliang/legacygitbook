# Binary Indexed Tree

1. [good video tutorial](https://www.youtube.com/watch?v=v_wj_mOAlig&t=233s)

2. Binary Indexed Tree的基本实现

```java
    // BIT是binary indexed tree的array, index和delta表示对original array的index元素 add delta value
    public void updateBIT(int[] BIT, int index, int delta) {
        // i index in BIT is larger than index in original array by 1
        for(int i = index + 1; i < BIT.length; i += i & (-i)) {
            BIT[i] += delta;
        }
    }

    // 表示original array中index从0 到 index(inclusive)的元素的sum
    public int sumBIT(int[] BIT, int index) {
        int sum = 0;
        // i index in BIT is larger than index in original array by 1
        for(int i = index + 1; i > 0; i -= i & (-i)) {
            sum += BIT[i];
        }
        return sum;
    }
```



