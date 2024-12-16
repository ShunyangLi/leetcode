# 40. Combination Sum II

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

 

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
```

 

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

## Solution

今天状态很不错，疯狂刷题。。。这个题就是回溯算法，添加一个剪枝就行，`cand[i] != cand[i - 1]`

```c++
class Solution {
public:

    void combine(vector<vector<int>>& res, vector<int>& path, vector<int>& candidates, int idx, int target) {
        int val = accumulate(path.begin(), path.end(), 0);

        if (val == target) res.push_back(path);
        if (val >= target) return;

        for (int i = idx; i < candidates.size(); i ++) {

            if (candidates[i] > target) continue;
            if (i > idx && candidates[i] == candidates[i - 1]) continue;

            path.push_back(candidates[i]);

            combine(res, path, candidates, i + 1, target);

            path.pop_back();
        }

    }


    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        auto res = vector<vector<int>>();
        auto path = vector<int>();

        sort(candidates.begin(), candidates.end());
        
        combine(res, path, candidates, 0, target);

        return res;
    }
};
```

