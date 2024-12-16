# 18. 4Sum

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**Example 2:**

```
Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
```

 

**Constraints:**

- `1 <= nums.length <= 200`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`

## Solution

有很多种算法可以调用，但是懒，选择了暴力递归

```c++
class Solution {
    void combine(vector<vector<int>>& res, vector<int>& path, vector<int>& candidates, int idx, int target) {
        long long val = 0;
        for (int const& v : path) val += v;

        if (val == target && path.size() == 4) res.push_back(path);
        if (path.size() >= 4) return;


        for (int i = idx; i < candidates.size(); i ++) {
            if (i > idx && candidates[i] == candidates[i - 1]) continue;
            path.push_back(candidates[i]);
            combine(res, path, candidates, i + 1, target);
            path.pop_back();
        }
    }

public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());

        auto res = vector<vector<int>>();
        auto path = vector<int>();

        if (nums.size() < 4) return res;

        combine(res, path, nums, 0, target);

        return res;

    }
};
```

