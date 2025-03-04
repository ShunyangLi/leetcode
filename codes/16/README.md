# 16. 3Sum Closest

Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.

Return *the sum of the three integers*.

You may assume that each input would have exactly one solution.

 

**Example 1:**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Example 2:**

```
Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
```

## Solution

和3sum那道题差不多，只是找到最接近的就行

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());

        int res = nums[0] + nums[1] + nums[2];

        int left = 0, right = 0;

        for (int i = 0; i < nums.size() - 2; i ++) {

            left = i + 1;
            right = nums.size() - 1;

            while (left < right) {
                int num = nums[i] + nums[left] + nums[right];

                if (abs(num - target) < abs(res - target)) res = num;

                if (num > target) right --;
                else left ++;
            }

        }

        return res;
    }
};
```
