# Union Find

1. 参考 [Link](http://zxi.mytechroad.com/blog/data-structure/sp1-union-find-set/)

```java
/*
复杂度
Find: O(ɑ(n))* ≈ O(1)  (Amortized running time)
Union: O(ɑ(n))* ≈ O(1)
Space: O(n)

Without optimization: Find: O(n)
*/ 
class UnionFindSet {
  private int[] parents_;
  private int[] ranks_;

  public UnionFindSet(int n) {
      parents_ = new int[n + 1];
      ranks_ = new int[n + 1];
      for (int i = 0; i < parents_.length; ++i) {
          parents_[i] = i;
          ranks_[i] = 1;
      }
  }

  public boolean Union(int u, int v) {
      int pu = Find(u);
      int pv = Find(v);
      if (pu == pv) return false;

      // Optimization: Union by ranks
      if (ranks_[pv] > ranks_[pu])
          parents_[pu] = pv;           
      else if (ranks_[pu] > ranks_[pv])
          parents_[pv] = pu;
      else {
          parents_[pv] = pu;
          ranks_[pu] += 1;
      }

      return true;
  }

  public int Find(int u) {
      while (parents_[u] != u) {
          // Optimization： Path compression
          parents_[u] = parents_[parents_[u]];
          u = parents_[u];
      }
      return u;
  }
}
```



