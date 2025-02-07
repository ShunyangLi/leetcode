# 1248. Count Number of Nice Subarrays

Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.

Return *the number of **nice** sub-arrays*.

 

**Example 1:**

```
Input: nums = [1,1,2,1,1], k = 3
Output: 2
Explanation: The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].
```

**Example 2:**

```
Input: nums = [2,4,6], k = 1
Output: 0
Explanation: There are no odd numbers in the array.
```

**Example 3:**

```
Input: nums = [2,2,2,1,2,2,1,2,2,2], k = 2
Output: 16
```

## Solution

我们可以转换为 **至多 `k` 个奇数的子数组个数**：

- `f(k) = "最多 k 个奇数的子数组数"`
- **最终答案 = `f(k) - f(k-1)`**

#### **1. 定义**

- **`atMost(k)`**: 计算**最多**包含 `k` 个奇数的子数组的个数，即**子数组中奇数的个数 ≤ k**。
- **`exact(k)`**: 计算**恰好**包含 `k` 个奇数的子数组的个数。

#### **2. 关系**

我们可以通过 `atMost(k)` 计算 `exact(k)`：

$$
\text{exact}(k) = \text{atMost}(k) - \text{atMost}(k-1) 
$$


解释：

- `atMost(k)`: 计算最多 `k` 个奇数的子数组（包括 `0, 1, 2, ..., k` 个奇数）。
- `atMost(k-1)`: 计算最多 `k-1` 个奇数的子数组（包括 `0, 1, 2, ..., k-1` 个奇数）。
- **两者相减，剩下的就是恰好 `k` 个奇数的子数组数目。**

```c++
class Solution {
public:
    int atmost(vector<int>& nums, int k) {
        int res = 0, left = 0;

        for (int i = 0; i < nums.size(); i ++) {

            k -= nums[i] % 2;

            while (k < 0) k += nums[left ++] % 2;

            res += i - left + 1;
        }

        return res;
    }


    int numberOfSubarrays(vector<int>& nums, int k) {
        return atmost(nums, k)- atmost(nums, k - 1);
    }
};
```
