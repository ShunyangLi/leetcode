# 229. Majority Element II

Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

 

**Example 1:**

```
Input: nums = [3,2,3]
Output: [3]
```

**Example 2:**

```
Input: nums = [1]
Output: [1]
```

**Example 3:**

```
Input: nums = [1,2]
Output: [1,2]
```

 

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `-109 <= nums[i] <= 109`

 

**Follow up:** Could you solve the problem in linear time and in `O(1)` space?

## Solution

### S1

使用`hashmap`能轻松解决，或者`sort`都行，但是空间复杂度和时间复杂度不达标。

```c++
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        if (nums.size() <= 1) return nums;

        auto maps = unordered_map<int, int>();
        auto res = vector<int>();

        for (int const& num : nums) {
            maps[num] += 1;
        }

        auto marked = unordered_map<int, bool>();
        for (int const& num : nums) {
            if (marked[num]) continue;

            marked[num] = true;

            if (maps[num] > int(nums.size() / 3)) res.push_back(num);
        }

        return res;
    }
};
```

### S2

前面有一个类似的题，可以使用**Boyer-Moore 投票算法**

**问题理解：**

- 找到所有出现次数超过 `⌊n / 3⌋` 的元素。
- 注意最多可能有 **两个候选元素** 满足条件。

**核心思想：**

- 扩展 Boyer-Moore 投票算法，可以用于寻找 **两个候选的众数**。
- 通过两轮遍历：
  - **第一轮：** 找到最多两个候选的众数。
  - **第二轮：** 验证候选的众数是否真的满足出现次数超过 `⌊n / 3⌋`。

**算法步骤：**

**第一轮：寻找候选者**

- 用两个变量 `candidate1` 和 `candidate2` 记录候选众数。
- 用两个计数器 `count1` 和 `count2` 记录各自的计数。
- 遍历数组：
  - 如果当前元素等于 `candidate1`，增加 `count1`。
  - 如果当前元素等于 `candidate2`，增加 `count2`。
  - 如果 `count1` 为 0，将当前元素作为 `candidate1`，并将 `count1` 设置为 1。
  - 如果 `count2` 为 0，将当前元素作为 `candidate2`，并将 `count2` 设置为 1。
  - 否则，同时减少 `count1` 和 `count2`。

**第二轮：验证候选者**

- 用一次遍历统计 `candidate1` 和 `candidate2` 的实际出现次数。
- 检查它们是否满足条件（超过 `⌊n / 3⌋` 次）。

```c++
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        if (nums.size() <= 1) return nums;

        int cand1 = INT_MAX, cand2 = INT_MAX, c1 = 0, c2 = 0;

        for (int const& num : nums) {
            if (num == cand1) {
                c1 ++;
            } else if (num == cand2) {
                c2 ++;
            } else if (c1 ==  0) {
                cand1 = num;
                c1++;
            } else if (c2 == 0) {
                cand2 = num;
                c2++;
            } else {
                c1 --;
                c2 --;
            }
        }

        cout << cand1 << endl;

        int cc1 = 0, cc2 = 0;
        auto res = vector<int>();

        for (int const& num : nums) {
            if (num == cand1) cc1 ++;
            if (num == cand2) cc2 ++;
        }

        if (cc1 > int(nums.size() / 3)) res.push_back(cand1);
        if (cc2 > int (nums.size() / 3)) res.push_back(cand2);
        
        return res;
    }
};
```

