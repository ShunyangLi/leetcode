# 128. Longest Consecutive Sequence

Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

```

**Example 2:**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

```

**Constraints:**

- `0 <= nums.length <= 105`
- `109 <= nums[i] <= 109`

## Solution

这个题是通过union find来实现的，因为要求的是连续的元素，我们可以结合hashmap和union find来实现，hashmap是为了判断当前点的邻居是不是存在于数组中，并且判断是不是重复的元素。

```c
class UnionFind {
private:
    vector<int> root;
    vector<int> rank;
    int cnt;

public:
    explicit UnionFind(int n) {
        for (int i = 0; i < n; i++) {
            root.push_back(i);
            rank.push_back(1);
        }

        cnt = n;
    }

    auto union_(int x, int y) -> void {
        // something
        int root_x = find(x);
        int root_y = find(y);

        if (root_x == root_y) return;

        if (rank[root_x] <= rank[root_y]) {
            root[root_x] = root_y;
            rank[root_y] += rank[root_x];
        } else {
            root[root_y] = root_x;
            rank[root_x] += rank[root_y];
        }

        cnt --;
    }

    auto find(int x) -> int {
        if (root[x] != x) root[x] = find(root[x]);

        return root[x];
    }

    auto get_rank_max() -> int {
        return *max_element(rank.begin(), rank.end());
    }

    auto is_connected(int x, int y) -> bool {
        return find(x) == find(y);
    }

    [[nodiscard]] auto get_cnt() const -> int {
        return cnt;
    }
};

class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;

        auto uf = UnionFind(nums.size());

        auto maps = unordered_map<int, int>();

        // the number can be count twice, with one loop can reduce that
        for (int i = 0; i < nums.size(); i ++) {
            int num = nums[i];

            if (maps.find(num) != maps.end()) continue;
            maps[num] = i;

            if (maps.find(num - 1) != maps.end()) {
                uf.union_(i, maps[num - 1]);
            }

            if (maps.find(num + 1) != maps.end()) {
                uf.union_(i, maps[num + 1]);
            }
        }

        return uf.get_rank_max();
    }
};
```