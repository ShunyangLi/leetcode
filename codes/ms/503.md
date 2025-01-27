# 503. Next Greater Element II

Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return *the **next greater number** for every element in* `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

**Example 1:**

```
Input: nums = [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2;
The number 2 can't find next greater number.
The second 1's next greater number needs to search circularly, which is also 2.

```

**Example 2:**

```
Input: nums = [1,2,3,4,3]
Output: [2,3,4,-1,4]

```

## Solution

这个题和[739](https://shunyangli.github.io/leetcode/codes/739/)比较类似，都是找到下个比当前数字大的，但是不同的是，它这个是可以循环的。但其实就是扫两遍，从前往后的扫，第一遍是为了找到比当前数字更大的，第二遍是为了找到最大值后面那些数字的结果。

```c
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        stack<pair<int, int>> stack;
        auto res = vector<int>(nums.size(), -1);

        int n = nums.size();

        for (int i = 0; i < n * 2; i ++) {

            while (!stack.empty()) {
                auto it = stack.top();

                if (it.first >= nums[i % n]) break;

                res[it.second] = nums[i % n];

                stack.pop();
            }

            if (i < nums.size()) stack.emplace(nums[i], i);
        }

        return res;
    }
};
```