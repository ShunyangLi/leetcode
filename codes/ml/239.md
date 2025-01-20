# 239. Sliding Window Maximum (hard)

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

## Solution

这个题就是使用单调队列来解决的。

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> vec;
        vector<int> res;
        res.resize(nums.size() - k + 1);

        int sidx = 0;
        int max_val = INT_MIN;

        for (int i = 0; i < nums.size(); i ++) {
            sidx = i - k + 1;

            while (!vec.empty() && i - vec.front() >= k) {
                vec.pop_front();
            }

            while (!vec.empty() && nums[vec.back()] < nums[i]) {
                vec.pop_back();
            }

            vec.push_back(i);

            if (sidx >= 0) res[sidx] = nums[vec.front()];
            
        }

        return res;

    }
};
```