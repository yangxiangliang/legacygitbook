# Linked List

1. Slow and Fast pointers
   1. 可以找cycle
   2. 可以找linked list 的中点
2. 看到题目时，先想List有没有可能有cycle这种特殊情况
3. 有时候需要用 dummy node，dummy.next 作为return的List Head
4. 注意pre node, cur node的使用，有时候需要记录当前node，之前一个pre node
5. Reverse List通常有2种方法/写法

   1. 如果是reverse整个List，用一个pre node，初始为Null，把cur node的next设置为pre，依次往后
   2. 如果是reverse整个List中的subList，可以参见 \[[1.12](/212-reverse-linked-list-ii.md)\]/\[[1.17](/117-reverse-nodes-in-k-group.md)\] 中方法



