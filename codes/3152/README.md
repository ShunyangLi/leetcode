# 3152. Special Array II

An array is considered **special** if every pair of its adjacent elements contains two numbers with different parity.

You are given an array of integer `nums` and a 2D integer matrix `queries`, where for `queries[i] = [fromi, toi]` your task is to check that 

subarray

 `nums[fromi..toi]` is **special** or not.



Return an array of booleans `answer` such that `answer[i]` is `true` if `nums[fromi..toi]` is special.

 

**Example 1:**

**Input:** nums = [3,4,1,2,6], queries = [[0,4]]

**Output:** [false]

**Explanation:**

The subarray is `[3,4,1,2,6]`. 2 and 6 are both even.

**Example 2:**

**Input:** nums = [4,3,1,6], queries = [[0,2],[2,3]]

**Output:** [false,true]

**Explanation:**

1. The subarray is `[4,3,1]`. 3 and 1 are both odd. So the answer to this query is `false`.
2. The subarray is `[1,6]`. There is only one pair: `(1,6)` and it contains numbers with different parity. So the answer to this query is `true`.

 

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`
- `1 <= queries.length <= 105`
- `queries[i].length == 2`
- `0 <= queries[i][0] <= queries[i][1] <= nums.length - 1`

## Solution

### S1

直接暴力搜索query的范围区间，判断是不是有bug，如果有bug就标记错误，但是这种方法时间会超出预期。

```c++
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {
        auto res = vector<bool>(queries.size());
        int idx = 0;

        for (auto const& query : queries) {
            int s = query[0]; int e = query[1];

            bool is_odd = nums[s] % 2 == 0 ? false :  true;
            bool r = true;

            for (s = s + 1; s <= e; s ++) {
                bool t = nums[s] % 2 == 0 ? false :  true;

                if (is_odd == t) {
                    r = false;
                    break;
                } else {
                    is_odd = t;
                }
            }

            res[idx++] = r;
        }

        return res;
    }
};
```

### S2

我通过记录`nums`中每个发生错误的首地址，然后query的时候判断该区间是否包含该错误地址，如果包含则false。但是这种方法效率也不是最高的

```c++
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {

        auto err = vector<int>();

        if (nums.size() > 1) {
            for (int i = 1; i < nums.size(); i ++) {
                if (nums[i - 1] % 2 == nums[i] % 2) err.push_back(i - 1);
            }
        }

        auto res = vector<bool>(queries.size(), true);

        for (int i = 0; i < queries.size(); i ++) {
            int s = queries[i][0]; int e = queries[i][1];
            
            auto lower = std::lower_bound(err.begin(), err.end(), s);
            if (lower != err.end() && *lower < e) {
                res[i] = false;
            }
        }

        return res;
    }
};
```

### S3

GPT给的最优解，第一步是类似的，先建立一个数组来记录是否bug，如果bug则标记1。第二部是通过建立prefix的方式，来记录当0-当前数字中有多少个bug。这样query的时候只要需要判断`query[0]`到`query[1]`中包含多少个bug就行。如果是0则表示这个数组是valid。

```c++
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {

        int n = nums.size();
        std::vector<int> isError(n, 0);

        // Mark errors
        for (int i = 1; i < n; i++) {
            if (nums[i - 1] % 2 == nums[i] % 2) {
                isError[i - 1] = 1;
            }
        }

        // Build prefix sum
        std::vector<int> prefixSum(n + 1, 0);
        for (int i = 0; i < n; i++) {
            prefixSum[i + 1] = prefixSum[i] + isError[i];
        }

        // Answer queries
        std::vector<bool> res;
        for (const auto& query : queries) {
            int s = query[0], e = query[1];
            // Check if there's any error in the range [s, e)
            res.push_back(prefixSum[e] - prefixSum[s] == 0);
        }

        return res;
    }
};
```

