# 1646. Get Maximum in Generated Array
You are given an integer `n`. A **0-indexed** integer array `nums` of length `n + 1` is generated in the following way:

- `nums[0] = 0`
- `nums[1] = 1`
- `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`
- `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`

Return *the **maximum** integer in the array* `nums`.

 

**Example 1:**

```
Input: n = 7
Output: 3
Explanation: According to the given rules:
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
Hence, nums = [0,1,1,2,1,3,2,3], and the maximum is max(0,1,1,2,1,3,2,3) = 3.
```

**Example 2:**

```
Input: n = 2
Output: 1
Explanation: According to the given rules, nums = [0,1,1]. The maximum is max(0,1,1) = 1.
```

**Example 3:**

```
Input: n = 3
Output: 2
Explanation: According to the given rules, nums = [0,1,1,2]. The maximum is max(0,1,1,2) = 2.
```

## Solution

好像主要是题意不好理解。

你需要生成一个数组 `nums`，其中：

- `nums[0]=0`
- `nums[1]=1`(如果 `n >= 1`）。
- 对于 `2 <= i <= n`，根据以下规则计算：
  - 如果 `i` 是偶数：`nums[i]=nums[i/2]`。
  - 如果 `i`是奇数：`nums[i]=nums[i/2]+nums[i/2+1]`。

最后，返回生成的数组中最大的值。

```c++
class Solution {
public:
    int getMaximumGenerated(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;

        auto nums = vector<int>();

        nums.push_back(0);
        nums.push_back(1);

        int max_val = 1;

        for (int i = 2; i <= n; i ++) {
            int idx = int (i / 2);

            int val = i % 2 == 0 ? nums[idx] : nums[idx] + nums[idx + 1];
            nums.push_back(val);

            max_val = max_val > val ? max_val : val;
        }

        return max_val;
    }
};
```

空间复杂度还能进一步优化。