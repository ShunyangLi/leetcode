# 215. Kth Largest Element in an Array

Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

 

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

**Example 2:**

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

 

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

## Solution

最简单的方法就是使用`priority queue`来解决这个问题，但是效率不是很高。可以考虑使用`QuickSelect`算法。等看完来更新这个算法。

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        auto cmp = [](const int& a, const int& b) {
            return a < b;
        };

        priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);

        for (int const& num : nums) pq.push(num);

        int val = 0;

        while (!pq.empty()) {
            k --;
            if(k == 0) {
                val = pq.top();
                break;
            }
            pq.pop();
        }

        return val;
    }
};
```
### S2
另外一种方法就是通过`dfs`来实现的，其实也不难，就只需要从边缘的点出发，然后判断是不是`O`如果是则进行dfs遍历找到所有不发生改变的`O`。

```c++
class Solution {
public:
    void dfs(int i, int j, int rows, int cols, vector<vector<char>>& board, vector<vector<bool>>& maps) {
        stack<pair<int, int>> stack;

        stack.emplace(i, j);
        
        while (!stack.empty()) {
            auto it = stack.top();
            stack.pop();

            int x = it.first, y = it.second;
            maps[x][y] = true;

            if (x - 1 >= 0 && board[x - 1][y] == 'O' && !maps[x-1][y]) stack.emplace(x-1, y);
            if (x + 1 < rows && board[x + 1][y] == 'O' && !maps[x + 1][y]) stack.emplace(x + 1, y);
            if (y - 1 >= 0 && board[x][y - 1] == 'O' && !maps[x][y-1]) stack.emplace(x, y-1);
            if (y + 1 < cols && board[x][y+1] == 'O' && !maps[x][y+1]) stack.emplace(x, y+1);

        }
    }

    void solve(vector<vector<char>>& board) {
        auto maps = vector<vector<bool>>(board.size());
        
        for (int i = 0; i < board.size(); i ++) maps[i].resize(board[i].size(), false);


        int rows = board.size();
        int cols = board[0].size();

        for (int i = 0; i < rows; i ++) {
            if (i == 0 || i == rows - 1) {
                // check all cols
                for (int j = 0; j < cols; j ++) {
                    if (board[i][j] == 'O') {
                        // DFS
                        dfs(i, j, rows, cols, board, maps);
                    }
                }
            } else {
                // check the first and last elements
                int left = 0;
                int right = cols - 1;

                if (board[i][left] == 'O') dfs(i, left, rows, cols, board, maps);
                if (board[i][right] == 'O') dfs(i, right, rows, cols, board, maps);
            }
        }

        for (int i = 0; i < maps.size(); i ++) {
            for (int j = 0; j < maps[0].size(); j ++) {
                if (maps[i][j]) board[i][j] = 'O';
                else board[i][j] = 'X';
            }
        }

    }
};
```