# 2593. Find Score of an Array After Marking All Elements

You are given an array `nums` consisting of positive integers.

Starting with `score = 0`, apply the following algorithm:

- Choose the smallest integer of the array that is not marked. If there is a tie, choose the one with the smallest index.
- Add the value of the chosen integer to `score`.
- Mark **the chosen element and its two adjacent elements if they exist**.
- Repeat until all the array elements are marked.

Return *the score you get after applying the above algorithm*.

 

**Example 1:**

```
Input: nums = [2,1,3,4,5,2]
Output: 7
Explanation: We mark the elements as follows:
- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [2,1,3,4,5,2].
- 2 is the smallest unmarked element, so we mark it and its left adjacent element: [2,1,3,4,5,2].
- 4 is the only remaining unmarked element, so we mark it: [2,1,3,4,5,2].
Our score is 1 + 2 + 4 = 7.
```

**Example 2:**

```
Input: nums = [2,3,5,1,3,2]
Output: 5
Explanation: We mark the elements as follows:
- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [2,3,5,1,3,2].
- 2 is the smallest unmarked element, since there are two of them, we choose the left-most one, so we mark the one at index 0 and its right adjacent element: [2,3,5,1,3,2].
- 2 is the only remaining unmarked element, so we mark it: [2,3,5,1,3,2].
Our score is 1 + 2 + 2 = 5.
```

## Solution

使用priority queue的思路就行，最小的数字放在最前面。`pq`存的数据包含数字和对应的index就行。

### S1

```c++
class Solution {
public:
    long long findScore(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];


        auto marked = vector<bool>(nums.size(), false);

        auto pq = std::vector<pair<int, int>>();
        for (int i = 0; i < nums.size(); i ++) {
            pq.emplace_back(nums[i], i);
        }

        std::sort(pq.begin(), pq.end(), [](const std::pair<int, int>& a, const std::pair<int, int>& b) {
            return a.first > b.first || (a.first == b.first && a.second > b.second);
        });


        long long res = 0;

        while (!pq.empty()) {
            auto it = pq.back();
            pq.pop_back();

            if (marked[it.second]) continue;
            // visited

            marked[it.second] = true;

            if (it.second == 0) {
                marked[it.second + 1] = true;
            } else if (it.second == nums.size() - 1) {
                marked[it.second - 1] = true;
            } else {
                marked[it.second - 1] = true;
                marked[it.second + 1] = true;
            }

            res += it.first;
        }

        return res;
        
    }
};
```

### S2

```c++
class Compare {
public:
    bool operator()(pair<int, int> below, pair<int, int> above)
    {
        if (below.first > above.first) {
            return true;
        }
        else if (below.first == above.first
                 && below.second > above.second) {
            return true;
        }

        return false;
    }
};

class Solution {
public:
    long long findScore(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];


        auto marked = vector<bool>(nums.size(), false);
        auto pq = priority_queue<pair<int, int>, vector<pair<int, int>>, Compare>();

        for (int i = 0; i < nums.size(); i ++) {
            pq.emplace(nums[i], i);
        }

        long long res = 0;

        while (!pq.empty()) {
            auto it = pq.top();
            pq.pop();

            if (marked[it.second]) continue;
            // visited

            marked[it.second] = true;

            if (it.second == 0) {
                marked[it.second + 1] = true;
            } else if (it.second == nums.size() - 1) {
                marked[it.second - 1] = true;
            } else {
                marked[it.second - 1] = true;
                marked[it.second + 1] = true;
            }

            res += it.first;
        }

        return res;
        
    }
};
```

