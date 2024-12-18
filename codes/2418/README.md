# 2418. Sort the People

You are given an array of strings `names`, and an array `heights` that consists of **distinct** positive integers. Both arrays are of length `n`.

For each index `i`, `names[i]` and `heights[i]` denote the name and height of the `ith` person.

Return `names` *sorted in **descending** order by the people's heights*.

 

**Example 1:**

```
Input: names = ["Mary","John","Emma"], heights = [180,165,170]
Output: ["Mary","Emma","John"]
Explanation: Mary is the tallest, followed by Emma and John.
```

**Example 2:**

```
Input: names = ["Alice","Bob","Bob"], heights = [155,185,150]
Output: ["Bob","Alice","Bob"]
Explanation: The first Bob is the tallest, followed by Alice and the second Bob.
```

 ## Solution

用`priority queue`就能解决，难度不大

```c++
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        auto cmp = [](const pair<string, int> &a, const pair<string, int> &b) {
            return a.second < b.second;
        };
        priority_queue<pair<string, int>, vector<pair<string, int>>, decltype(cmp)> pq(cmp);

        for (int i = 0; i < names.size(); i ++) {
            pq.emplace(names[i], heights[i]);
        }

        auto ans = vector<string>();
        while (!pq.empty()) {
            ans.push_back(pq.top().first);
            pq.pop();
        }
        
        return ans;
    }
};
```

