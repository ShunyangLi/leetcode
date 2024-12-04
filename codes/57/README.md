# 57. Insert Interval

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion*.

**Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

 

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

 

**Constraints:**

- `0 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 105`
- `intervals` is sorted by `starti` in **ascending** order.
- `newInterval.length == 2`
- `0 <= start <= end <= 105`

## Solution

和[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)基本上没啥区别，多了一个二分搜索，找到合适的插入位置。

```c++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int left = 0, right = intervals.size() - 1;
        int target = newInterval[0];

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (intervals[mid][0] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        intervals.insert(intervals.begin() + left, newInterval);
        
        vector<vector<int>> res;

        int idx = 0;

        while (idx < intervals.size()) {
            int val = intervals[idx][1];
            int j = idx + 1;

            while (j < intervals.size()) {
                if (intervals[j][0] > val) break;
                val = std::max(val, intervals[j][1]);
                j++;
            }

            res.emplace_back(vector<int>{intervals[idx][0], val});
            idx = j;
        }

        return res;
    }
};
```

