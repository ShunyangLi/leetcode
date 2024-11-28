# Union Find

**Union-Find**（又称**并查集**）是一种常用的数据结构，主要用于解决动态连通性问题，比如判断某些元素是否属于同一组，或合并两个组。

### 基本功能

1. **Find**：找到某个元素所属组的代表元素（也叫根节点）。
2. **Union**：将两个组合并成一个组。

### 工作原理

1. **初始化**：每个元素自成一组，`parent[i] = i`。
2. `Find` 操作：
   - 通过递归或迭代找到某个元素的根节点。
   - **路径压缩优化**：在查找时将路径上的节点直接指向根节点，减少后续操作的查找深度。
3. `Union` 操作：
   - 将两个组的根节点连接。
   - **按秩合并优化**：将节点数少的树挂到节点数多的树上，或根据深度决定如何合并，避免树变得过高。

```c++
class UnionFind {
   private:
    vector<int> root;
    int cnt;

   public:
    explicit UnionFind(int n) {
        for (int i = 0; i < n; i++) root.push_back(i);
        cnt = n;
    }

    auto union_(int x, int y) -> void {
        int root_x = find(x);
        int root_y = find(y);

        if (root_x != root_y) {
            root[root_x] = root_y;
            cnt--;
        }
    }

    auto find(int x) -> int {
        while (root[x] != x) {
            x = root[x];
        }

        return x;
    }

    auto is_connected(int x, int y) -> bool { return find(x) == find(y); }

    [[nodiscard]] auto get_cnt() const -> int { return cnt; }
};
```

```
0 <- 1 <- 2 <- 3 <- 4
```

在 **Union-Find** 中，最差的情况发生在没有任何优化时，当所有元素形成一个**链表结构**，查询复杂度会退化为 O(n) 如上图所示。

## Optimization

**路径压缩**：

- 在 `find` 操作时，将访问路径上的所有节点直接连接到根节点。
- 压缩后，查询树的高度趋向于 O(log n) 或更低 (O(1))。

```c++
class UnionFind {
   private:
    vector<int> root;
    int cnt;

   public:
    explicit UnionFind(int n) {
        for (int i = 0; i < n; i++) root.push_back(i);
        cnt = n;
    }

    auto union_(int x, int y) -> void {
        int root_x = find(x);
        int root_y = find(y);

        if (root_x != root_y) {
            root[root_x] = root_y;
            cnt--;
        }
    }

    auto find(int x) -> int {
        if (root[x] != x) root[x] = find(root[x]);

        return root[x];
    }

    auto is_connected(int x, int y) -> bool { return find(x) == find(y); }

    [[nodiscard]] auto get_cnt() const -> int { return cnt; }
};
```

在 `find` 方法中：

- 使用递归实现路径压缩：`root[x] = find(root[x]);`。
- 这会将节点 `x` 的父节点直接设置为根节点，从而减少树的深度。

这样的话上面图中`4, 3, 2, 1`的root都会直接指向`0`。这样的话查询效率会提升很多。
