# Topological Sort

1. 注意 topological sorting 不是针对所有graph都有用，只有针对 Directed Acyclic Graph\(DAG\) 才能用。基本思路如下：
   1. 同样用DFS，只不过DFS当前 vertice 的时候，不马上print vertice。用一个stack，利用其“先进后出”的性质，在对其所有adjacent vertices recursively 做完 topological sorting 后，再把当前vertice 压入 stack。即准备把某个vertice 压入stack的时候，必须保证其所有adjacent vertices 都已经被压入stack 。最后print处stack中的元素即可。
   2. 注意topological sorting 的结果可能不止一种情况。其第一个vertex 往往是 in-degree 等于 0 的那个 vertex。
   3. 可以用BFS的方法，关键是先把 in-degree 等于 0的vertex放入queue中，将其一个一个放入结果排序中。然后将其neighbors的in-degree的值减1，然后“一层一层”看是否有新增加的in-degree等于0的vertex，如果有，则继续放入queue中。具体方法可参考 [4.10 Alien Dictionary](/410-alien-dictionary.md)
   4. [Reference](https://www.geeksforgeeks.org/topological-sorting/) 



