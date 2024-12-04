# 56. Merge Intervals

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

 

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

**Example 2:**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

 

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

## Solution

先进行排序，值得注意的是先对数组的第一个数字进行排序，然后是第二位数字。排完序之后再计算一下cover就行，不难，情况可能多一点，可以先写基础代码，然后优化。

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        std::sort(intervals.begin(), intervals.end(), [](const std::vector<int>& a, const std::vector<int>& b) {
            if (a[0] != b[0]) {
                return a[0] < b[0];
            } else {
                return a[1] < b[1];
            }
        });

        auto res = vector<vector<int>>();

        int idx = 0;

        while (idx < intervals.size()) {
            int val = intervals[idx][1];

            int j = idx + 1;

            while (j < intervals.size()) {
                if (intervals[j][0] > val) {
                    break;
                } else {
                    if (intervals[j][1] > val) val = intervals[j][1];
                }

                j += 1;
            }

            if (j != idx + 1) {
                j --;
                
                auto tmp = vector<int>();
                tmp.push_back(intervals[idx][0]);

                // if (intervals[idx][1] > intervals[j][1]) tmp.push_back(intervals[idx][1]);
                // else tmp.push_back(intervals[j][1]);
                tmp.push_back(val);

                res.push_back(tmp);

                idx = j + 1;

                continue;
            } else {
                res.push_back(intervals[idx]);
            }

            idx += 1;
        }

        return res;
    }
};
```

**优化之后的：**

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        
        std::sort(intervals.begin(), intervals.end(), [](const std::vector<int>& a, const std::vector<int>& b) {
            return a[0] < b[0] || (a[0] == b[0] && a[1] < b[1]);
        });

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

