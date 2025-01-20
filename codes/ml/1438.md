# 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit

Given an array of integers `nums` and an integer `limit`, return the size of the longest **non-empty** subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`*.*

 

**Example 1:**

```
Input: nums = [8,2,4,7], limit = 4
Output: 2 
Explanation: All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 <= 4.
[8,2] with maximum absolute diff |8-2| = 6 > 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
[2] with maximum absolute diff |2-2| = 0 <= 4.
[2,4] with maximum absolute diff |2-4| = 2 <= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 > 4.
[4] with maximum absolute diff |4-4| = 0 <= 4.
[4,7] with maximum absolute diff |4-7| = 3 <= 4.
[7] with maximum absolute diff |7-7| = 0 <= 4. 
Therefore, the size of the longest subarray is 2.
```

**Example 2:**

```
Input: nums = [10,1,2,4,7,2], limit = 5
Output: 4 
Explanation: The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.
```

**Example 3:**

```
Input: nums = [4,2,2,2,4,4,2,2], limit = 0
Output: 3
```

## Solution

也是居于sliding window的思路来解决的，但是不同的是，需要快速得到sliding window里面的最大值和最小值，所以使用了两个单调队列：

- **最大值单调队列 (`maxDeque`)**：存储当前窗口中元素值，从大到小排列，队头是最大值。

- **最小值单调队列 (`minDeque`)**：存储当前窗口中元素值，从小到大排列，队头是最小值。

因为我们考虑的是`longest`所以我们只需要考虑window里面的最大值和最小值差值是多少，然后判断是否收缩窗口。

```c++
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        deque<int> maxD; // decreasing
        deque<int> minD; // increasing

        int left = 0;
        int res = 0;

        for (int i = 0; i < nums.size(); i ++) {
            // add or remove elements
            while (!maxD.empty() && nums[maxD.back()] < nums[i]) maxD.pop_back();
            maxD.push_back(i);

            while (!minD.empty() && nums[minD.back()] > nums[i]) minD.pop_back();
            minD.push_back(i);

            while (!maxD.empty() && !minD.empty() && nums[maxD.front()] - nums[minD.front()] > limit) {
                if (maxD.front() == left) maxD.pop_front();
                if (minD.front() == left) minD.pop_front();

                left ++;
                // shrink the window
            }

            res = res > i - left + 1 ? res : i - left + 1;
        }

        return res;
    }
};
```

有个讲解视频不错，可以参考一下：https://www.youtube.com/watch?v=V-ecDfY5xEw