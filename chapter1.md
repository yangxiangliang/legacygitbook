# Tree

1. 很多题可以套用DFS Recursion来处理，关键看root.left 是否可以用recursion function，root.right 是否可以用recursion function处理，然后将结果"拼"起来即可。**Example: **[**2.1.4**](/224-invert-binary-tree.md)
2. 如果需要level by level来处理的，通常可以用一个queue，BFS来处理。
3. 用global variable的情况\(**e.g 2.1.8**\)，通常也可以用另外一个function，然后不用global variable，将原有的global variable当做一个param parse到这个function中做recursion。这两种方法通常可以互换。
4. 在很多边际条件terminate的时候，比如在node是leaf的情况下需要terminate的时候，检查\(node.left == null && node.right == null\)，而不能再往下走一个level 检查\(node == null\)的情况，比如node.left != null && node.right == null的情况，node并不是leaf。**Example: **[**2.2.6**](/226-path-sum-ii.md)
5. 注意有时候DFS从root往leaf走的时候，有时候记录的List是share的，可能会在recursion中被改变，所以在recursion后需要remove掉此层新家的元素。**Example: **[**2.2.6**](/226-path-sum-ii.md)** \(比如此题中如果 List变成sum, 类似题目LeetCode 129中, sum 只是int，不会被share中修改，所以不会考虑remove 掉此层中新加的东西\)**。
6. 经常没有思路，比较复杂的题目是，可以想Recursive的方法，比较容易理解。**Example: **[**2.2.7**](/227-delete-node-in-a-bst.md)
7. **Bottom-Up**的思路有时候常用。**Example: **[**2.2.11**](/2211.md)** \(2\)**
8. 构建新的class作为helper function 返回的结果，有时候会更efficient。**Example:**[** 2.2.12**](/2212-largest-bst-subtree.md)** \(2\)**
9. 解题时，有时用global variable可以解决时，同理可以用一个helper function，把需要构建的这个global variable parse到helper function中。**Example: **[**2.2.27**](/chapter1/2227-binary-tree-preorder-traversal.md)



