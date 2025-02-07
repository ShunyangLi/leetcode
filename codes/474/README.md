# 474. Ones and Zeroes

You are given an array of binary strings `strs` and two integers `m` and `n`.

Return *the size of the largest subset of `strs` such that there are **at most*** `m` `0`*'s and* `n` `1`*'s in the subset*.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

 

**Example 1:**

```
Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.
```

**Example 2:**

```
Input: strs = ["10","0","1"], m = 1, n = 1
Output: 2
Explanation: The largest subset is {"0", "1"}, so the answer is 2.
```

## Solution

也是一种背包问题，但这个不同的地方在于多了一个条件，相当于是判断两个条件下的最大值。

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));

        for (string const& str : strs) {
            int z = 0, o = 0;

            for (char const& c : str) {
                if (c == '0') z ++;
                else o++;
            }

            for (int i = m; i >= z; i --) {
                for (int j = n; j >= o; j --) {
                    // i - z store the status if take this string
                    dp[i][j] = max(dp[i][j], dp[i - z][j - o] + 1);
                }
            }
        }

        return dp[m][n];
    }
};
```
