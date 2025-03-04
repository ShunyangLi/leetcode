# 454. 4Sum II

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

 

**Example 1:**

```
Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
Output: 2
Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**Example 2:**

```
Input: nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
Output: 1
```

## Solution

这个题目要求会简单一些，只要求统计数量，不要求具体的排列组合，所以可以把时间复杂度降到`O(^2)`，可以通过用hashmap记录前两个数组的排列组合的结果和数量，然后遍历剩下的数组，找到差值，这样就能得到总的数量了

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        auto maps = unordered_map<int, int>(0);

        for (int i = 0; i < nums1.size(); i ++) {
            for (int j = 0; j < nums2.size(); j ++) {
                int val = nums1[i] + nums2[j];

                if (maps.find(val) == maps.end()) maps[val] = 0;
                maps[val] ++;
            }
        }

        int cnt = 0;

        for (int i = 0; i < nums3.size(); i ++) {
            for (int j = 0; j < nums4.size(); j ++) {
                int val = nums3[i] + nums4[j];
                if (maps.find(-val) != maps.end()) cnt += maps[-val];
            }
        }

        return cnt;
    }
};
```

