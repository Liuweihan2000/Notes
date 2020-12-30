### 146. LRU缓存机制

创建双向链表来记录最近使用的节点列表

双向链表的时间复杂度由于单向链表，因为不需要线性查找

创建额外的head和tail节点可以避免边界情况的讨论

map中存放的应该是<Integer, ListNode>，这样可以直接对ListNode做操作而不用线性查找



### 684. 冗余连接 medium

这道题用并查集，要记住并查集这个数据结构的实现方法，其中比较比较重要的方法是find，它负责路径压缩+找到某个节点的祖先

```java
public class UnionFind {

    int[] ancestor;

    public UnionFind(int n) {
        ancestor = new int[n];
        for (int i = 0; i < n; i++)
            ancestor[i] = i;
    }

    public void union(int index1, int index2) {
        ancestor[find(index1)] = find(index2);
    }

    // 顺便把路径压缩算法实现了
    public int find(int index) {
        if (ancestor[index] != index) {
            ancestor[index] = find(ancestor[index]);
        }
        return ancestor[index];
    }

}
```



### 685. 冗余连接2 hard

这道题用了两个用于记录状态信息的数组parent和UnionFind类里的ancestor

它们都是用来记录上级的信息的，不同的是parent记录的是某个节点的直属上级节点，而ancestor记录的是某个节点的最高上级节点，这有点类似于路径压缩算法，这个算法的实现过程通过调用UnionFind类中的find顺便实现了

这样就可以用parent来判断是否发生conflict，因为从parent可以推导出这个节点拥有了两个直属上级节点，而用ancestor来判断是否出现环，因为两个节点拥有了相同的祖先

这两个数组的名字起的好，顾名思义即可



