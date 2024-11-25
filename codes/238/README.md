# 238. **Product of Array Except Self**

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

**Constraints:**

- `2 <= nums.length <= 105`
- `30 <= nums[i] <= 30`
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

## Solution

这个问题给出的提示 `prefix` 和 `suffix` ，那么根据这个提示我们可以得知这个题是需要前后遍历的算。并且该问题要求时间复杂度是 `O(n)` ，我们可以采取左右两边遍历的方法。

根据这个问题本身的要求，算出该数组所有数字的乘积，但是不包含当前数字。首先我们可以使用 `prefix` 从左到右遍历一下：

```jsx
nums: 1 2 3 4
pre:  1 1 2 6
```

我们可以观察到从左到右遍历的话， `nums` 中每个索引对应的数据是前面几个数字的乘积，这样的话我们可以得知从左到右的乘积是多少了。从右往左的时候，我们需要用当前数字乘以上一轮的结果，这样我们就能得到最后结果了。 `prefix` 记录的是从左到右的乘积， `sufix` 记录的是从右到左的乘积（不包含自身）。
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        auto ans = vector<int>(nums.size(), 1);

        int prefix = 1;
        for (int i = 0; i < nums.size(); i ++) {
            ans[i] = prefix;
            prefix *= nums[i];
        }

        int suffix = 1;
        for (int i = nums.size() - 1; i >= 0; i -- ) {
            ans[i] = suffix * ans[i];
            suffix *= nums[i];
        }

        return ans;
    }
};
```