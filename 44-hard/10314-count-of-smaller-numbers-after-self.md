# 315. Count of Smaller Numbers After Self

> You are given an integer array _nums_ and you have to return a new _counts_ array. The _counts_ array has the property where
>
> `counts[i]`is the number of smaller elements to the right of `nums[i]`.
>
> **Example:**
>
> ```
> Given nums= [5, 2, 6, 1]
>
> To the right of 5 there are 2 smaller elements (2 and 1).
> To the right of 2 there is only 1 smaller element (1).
> To the right of 6 there is 1 smaller element (1).
> To the right of 1 there is 0 smaller element.
> ```
>
> Return the array `[2, 1, 1, 0]`.

### 思路 1\)

这道题目的brute force方法很容易想到，就是找到i点，然后数i点右边有多少个小于其值的点即可，复杂度为O\(N^2\)。显然这不是最优解，所以我们应该寻找更好的方法。

第一种方法想到能不能把原来的数组和sorted的数组联系，因为在sorted的数组中找比某值小的值的个数是很容易的。而且这里只是找i右边比i小的值的个数。想到可以从nums的右边开始往左，那每个值一次insert到1个sorted的数组中，i 右边的比i的值小的个数就是i在sorted的array中插入的index的值。为了efficient，这里insert的时候用binary search来寻找insert的index，因为插入的array是sorted的。

### Code 1\)

```java
public class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> sorted = new ArrayList();
        List<Integer> rst = new ArrayList();
        for(int i = nums.length - 1; i >= 0; i --) {
            int index = binarySearchIndex(sorted, nums[i]);
            sorted.add(index, nums[i]);
            rst.add(0, index);
        }
        return rst;
    }

    public int binarySearchIndex(List<Integer> list, int target) {
        if(list.size() == 0) return 0;
        int start = 0, end = list.size() - 1;
        if(list.get(end) < target) return end + 1; // handle corner case
        if(list.get(start) >= target) return start; // handle corner case

        while(start < end) {
            int mid = start + (end - start) / 2;
            if(target > list.get(mid)) start = mid + 1;
            else end = mid;
        }
        return end;
    }
}
```

### 思路 2）

第二种方法是考虑Binary Search Tree，从int\[\] nums右边到左边构造binary search tree，但是这里的tree node需要加一些field才能更efficient，加1个int leftSum，表示该node 的left subtree一共的node的个数，因为其left subtree的node的值都是小于其的。除此之外，还需要加1个int count，因为同样的value可能有重复，count表示该value的值出现的次数，只出现一次的node的count值则是1。然后从nums 右到左insert每个node的时候，也可以计算小于该node的个数，即加入答案中。

### 复杂度

如果tree是balanced的情况，那么insert的complexity是log\(N\), 总的time complexity是O\(log\(N\) \* N\)

但是worst case，tree可能不是balanced，是1个长的LinkedList，所以总的time complexity是O\(N ^ 2\)

### Code 2\)

```java
public class Solution {
    class TreeNode {
        int val, leftSum, count;
        TreeNode left, right;
        TreeNode(int val) {
            this.val = val;
        }
    }

    public List<Integer> countSmaller(int[] nums) {
        List<Integer> rst = new ArrayList();
        if(nums.length == 0) return rst;

        // add root to binary search tree
        TreeNode root = new TreeNode(nums[nums.length-1]);
        for(int i = nums.length - 1; i >= 0; i --) {
            rst.add(0, insert(root, nums[i]));
        }
        return rst;
    }

    public int insert(TreeNode root, int num) {
        int sum = 0; // sum 则是有多少小于num值的node
        while(root.val != num) {
            if(num > root.val) {
                // 这里只有往右走的时候才增加sum的值
                sum += root.leftSum + root.count;
                if(root.right == null) root.right = new TreeNode(num);
                root = root.right;
            } else {
                if(root.left == null) root.left = new TreeNode(num);
                // 注意root.leftSum要增加，因为值为num的node需要加到root的left subtree中
                root.leftSum ++;
                root = root.left;
            }
        }
        // 因为这里root.val == num, 则值为num的node的个数+1，root.count的初始值为0
        root.count ++;
        return sum + root.leftSum;
    }
}
```

### 思路 3）

这里的思路比较高级，就是用BIT，binary indexed tree。首先将input的int\[\] nums需要sort一遍，然后用hashMap记录下各个值和其在sorted array中的index的对应情况\(不用特别处理duplicates的情况，随便将duplicate value的值和任何一个duplicate 的index对应的话，该方法都适用\)。然后构造int\[\] BIT数组，BIT的index与sorted array中的index是相对应的，BIT\[i\] 表示 sorted array中index为i的那个值到目前为止出现的次数。所以从input unsorted int\[\] nums 开始，从右往左，假设nums\[i\]在sorted array中的index对应的为k，那么到目前为止比nums\[k\]的小的值的个数即是已出现的: sortedArray\[0\], sortedArray\[1\], ....sortedArray\[k-1\] 的次数之和。即求int\[\] BIT的index 到K的preSum，转化为了Binary Indexed Tree求和的问题。然后同时要BIT\[k\]的值加1，因为现在这个值又出现了1次。

### 复杂度

binary indexed tree的update和sum的complexity是log\(N\)，总的time complexity是O\(log\(N\) \* N\)

### Code 3\)

```java
public class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> rst = new ArrayList();
        if(nums.length == 0) return rst;

        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        HashMap<Integer, Integer> map = new HashMap();
        for(int i = 0; i < sorted.length; i ++) map.put(sorted[i], i);

        // size of BIT is larger than size of original array by 1
        int[] BIT = new int[nums.length+1];
        for(int i = nums.length - 1; i >= 0; i --) {
            rst.add(0, sumBIT(BIT, map.get(nums[i]) - 1));
            updateBIT(BIT, map.get(nums[i]), 1);
        }
        return rst;
    }

    public void updateBIT(int[] BIT, int index, int delta) {
        // i index in BIT is larger than index in original array by 1
        for(int i = index + 1; i < BIT.length; i += i & (-i)) {
            BIT[i] += delta;
        }
    }

    public int sumBIT(int[] BIT, int index) {
        int sum = 0;
        // i index in BIT is larger than index in original array by 1
        for(int i = index + 1; i > 0; i -= i & (-i)) {
            sum += BIT[i];
        }
        return sum;
    }
}
```



