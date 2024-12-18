# 130. Surrounded Regions

You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: The region is surrounded with `'X'` cells if you can **connect the region** with `'X'` cells and none of the region cells are on the edge of the `board`.

A **surrounded region is captured** by replacing all `'O'`s with `'X'`s in the input matrix `board`.

 

**Example 1:**

**Input:** board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

**Output:** [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

**Example 2:**

**Input:** board = [["X"]]

**Output:** [["X"]]

## Solution

这个题大概意思就是当`O`在网格的边缘地区则不会被`X`同化，如果不在边缘的`O`能和在边缘的`O`连接的话则不会被同化。其他的则都会被同化成`X`。

### S1

可以通过边缘`O`出发做dfs找到和它所有关联的`O`。

### S2

通过`union find`来解决

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

    void union_(int x, int y) {
        int root_x = find(x);
        int root_y = find(y);

        if (root_x != root_y) {
            root[root_x] = root_y;
            cnt--;
        }
    }

    int find(int x) {
        if (root[x] != x) root[x] = find(root[x]);

        return root[x];
    }

    bool is_connected(int x, int y) { return find(x) == find(y); }

    int get_cnt() const { return cnt; }
};

class Solution {
public:
    int node(int i, int j, int cols) {
        return i * cols + j;
    }

    void solve(vector<vector<char>>& board) {
        if (board.empty()) return;

        auto size = board.size() * board[0].size();

        auto uf = UnionFind(size + 1);

        for (int i = 0; i < board.size(); i ++) {
            for (int j = 0; j < board[i].size(); j++) {

                if (board[i][j] == 'O') {
                    if (i == 0 || i == board.size() - 1 || j == board[i].size() - 1 || j == 0) {
                        uf.union_(node(i, j, board[0].size()), size); 
                    } else {
                        if(board[i-1][j] == 'O')  uf.union_(node(i,j, board[0].size()), node(i-1,j, board[0].size()));
                        if(board[i+1][j] == 'O')  uf.union_(node(i,j, board[0].size()), node(i+1,j, board[0].size()));
                        if(board[i][j-1] == 'O')  uf.union_(node(i,j, board[0].size()), node(i, j-1, board[0].size()));
                        if( board[i][j+1] == 'O')  uf.union_(node(i,j, board[0].size()), node(i, j+1, board[0].size()));
                    }
                }
                
            }
        }

        for (int i = 0; i < board.size(); i ++) {
            for (int j = 0; j < board[i].size(); j++) {
                if (uf.is_connected(node(i, j, board[0].size()), size)) board[i][j] = 'O';
                else board[i][j] = 'X';
            }
        }
    }
};
```

