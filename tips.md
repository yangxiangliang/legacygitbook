1. Java里面有 int Arrays.binarySearch\(int\[\] a, int key\)。a should be sorted here。It returns: index of the search key, if it is contained in the array; otherwise, \(-\(insertion point\) - 1\). The insertion point is defined as the point at which the key would be inserted into the array: the index of the first element greater than the key, or a.length if all elements in the array are less than the specified key. Note that this guarantees that the return value will be &gt;= 0 if and only if the key is found.
2. Java里写Heap的方法

   ```java
   // 以ListNode为例
   Queue<ListNode> minHeap = new PriorityQueue<ListNode>(heapSize, new Comparator<ListNode>() {
                  public int compare(ListNode l1, ListNode l2) {
                      return l1.val - l2.val;
                  }
              });
   ```

3. 有时候在原来的函数不方便写**Recursion**的时候，可以另外写一个**helper function **来解决此问题，因为**helper function **可以更自由的选择parse in的参数值。**Example: **[**3.1.3**](/313-add-and-search-word-data-structure-design.md)**，search 和 searchHelper function**

4. **Sliding Window **的方法运用。**Example: **[**10.3.1**](/44-hard/1031-longest-substring-with-at-most-k-distinct-characters.md)

5. **Binary Indexed Tree。Example: **[**10.3.2**](/44-hard/1032-range-sum-query-2d-mutable.md)

   1. [Video](https://www.youtube.com/watch?v=v_wj_mOAlig&t=231s)

   2. Implementation [example1](http://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/), [example2](https://github.com/jakobkogler/Algorithm-DataStructures/blob/master/BinaryIndexedTree/BinaryIndexedTree.py)

6. 最短路径的问题常常想到 **BFS** 来解决。**Example: **[**10.3.3**](/44-hard/1033-shortest-distance-from-all-buildings.md)** **

7. 最短路径用BFS时候，一个图形中可能有好几个start points，不用从每个start point开始做BFS，这样很慢。可以考虑把所有的start points 加入queue，然后一起做BFS操作。[**Example: 10.2.15 **](/43-medium/10215-walls-and-gates.md)

8. 有时候构造string的题目，用 char\[\] 比 String 或则 StringBuilder 更方便，特别是要改变char\[\] 中某些character值时。[**Example 10.2.17**](/43-medium/10217.md)

9. 有时候在 matrix中DFS的时候，不用按照常规方法，从\(i, j\) 开始往边界遍历。根据具体情况，可能可以采用 从边界开始往中间遍历DFS。[**Example: 10.2.19**](/43-medium/10219-pacific-atlantic-water-flow.md)

10. Java 里写Heap简洁的方法

    ```java
    // default comparator is for min heap
    Queue<Integer> minHeap = new PriorityQueue();
    // Collections.reverserOrder() comparator is for max heap
    Queue<Integer> maxHeap = new PriorityQueue<Integer>(Collections.reverseOrder());
    ```

11. ** Sliding Window **是常见方法，需要熟练运用。Example: [10.3.1](/44-hard/1031-longest-substring-with-at-most-k-distinct-characters.md), [10.3.8](/44-hard/1038-longest-substring-with-at-most-two-distinct-characters.md)

12. 注意 **HashSet**, **TreeSet**, **LinkedHashSet** 的运用。Example: [10.3.28](/44-hard/10328-lfu-cache.md) 中使用 **LinkedHashSet**

13. Two pointer 来解关于 substring的题目的模板，\[Link\]\([https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems](https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems)\). e.g. [Minimum Window Substring](/2017-minimum-window-substring.md); [Find all anagrams in a string](/2067-find-all-anagrams-in-a-string.md)



