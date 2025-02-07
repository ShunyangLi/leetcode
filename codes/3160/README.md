# 3160. Find the Number of Distinct Colors Among the Balls

You are given an integer `limit` and a 2D array `queries` of size `n x 2`.

There are `limit + 1` balls with **distinct** labels in the range `[0, limit]`. Initially, all balls are uncolored. For every query in `queries` that is of the form `[x, y]`, you mark ball `x` with the color `y`. After each query, you need to find the number of **distinct** colors among the balls.

Return an array `result` of length `n`, where `result[i]` denotes the number of distinct colors *after* `ith` query.

**Note** that when answering a query, lack of a color *will not* be considered as a color.

 

**Example 1:**

**Input:** limit = 4, queries = [[1,4],[2,5],[1,3],[3,4]]

**Output:** [1,2,2,3]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2024/04/17/ezgifcom-crop.gif)

- After query 0, ball 1 has color 4.
- After query 1, ball 1 has color 4, and ball 2 has color 5.
- After query 2, ball 1 has color 3, and ball 2 has color 5.
- After query 3, ball 1 has color 3, ball 2 has color 5, and ball 3 has color 4.

**Example 2:**

**Input:** limit = 4, queries = [[0,1],[1,2],[2,2],[3,4],[4,5]]

**Output:** [1,2,2,3,4]

**Explanation:**

**![img](https://assets.leetcode.com/uploads/2024/04/17/ezgifcom-crop2.gif)**

- After query 0, ball 0 has color 1.
- After query 1, ball 0 has color 1, and ball 1 has color 2.
- After query 2, ball 0 has color 1, and balls 1 and 2 have color 2.
- After query 3, ball 0 has color 1, balls 1 and 2 have color 2, and ball 3 has color 4.
- After query 4, ball 0 has color 1, balls 1 and 2 have color 2, ball 3 has color 4, and ball 4 has color 5.

## Solution

使用两个`hashmap`，第一个记录当前数字是否有颜色，第二个用来记录使用该颜色的有几个数字，如果没有人使用该颜色则从hashmap中删除，然后每一轮计算有多少个颜色就行。

```c++
class Solution {
public:
    vector<int> queryResults(int limit, vector<vector<int>>& queries) {
        auto vals = unordered_map<int, int>();
        auto colors = unordered_map<int, int>();

        auto res = vector<int>(queries.size(), 0);

        for (int i = 0; i < queries.size(); i ++) {

            const int x = queries[i][0];
            const int c = queries[i][1];

            if (vals.find(x) != vals.end()) {
                colors[vals[x]] -= 1;

                if (colors[vals[x]] == 0) colors.erase(vals[x]); 
            }

            colors[c] += 1;
            vals[x] = c;
            res[i] = colors.size();
        }

        return res;
    }
};
```

