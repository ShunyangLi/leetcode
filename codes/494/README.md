# 494. Target Sum

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

 

**Example 1:**

```
Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**Example 2:**

```
Input: nums = [1], target = 1
Output: 1
```

## Solution

#### **思路：转换为子集和问题**

1. 设所有取 `+` 号的数的和为 `P`，所有取 `-` 号的数的和为 `N`，则： P−N=target
2. 又因为所有元素的总和 `S = P + N`，可以推出： P=(S+target)/2
3. 这个问题就变成了 **在 `nums` 中找到一个子集，使其和等于 `(S + target) / 2`**，转化为了 **0/1 背包问题**。

这个问题是找多少种组合的可能性，不是找最大的，所以不需要考虑拿与不拿的问题，只需要把之前的排列组合的数量加一起就行。

```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = accumulate(nums.begin(), nums.end(), 0);

        if ((sum + target) % 2 != 0 || sum < abs(target)) return 0;

        int P = (sum + target) / 2; 
        vector<int> dp(P + 1, 0);
        dp[0] = 1;

        for (int num : nums) {
            for (int i = P; i >= num; i--) {
                dp[i] += dp[i - num];
            }
        }

        return dp[P];
    }
};
```

