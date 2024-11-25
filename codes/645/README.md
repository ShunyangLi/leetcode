# 645. Set Mismatch
You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in **repetition of one** number and **loss of another** number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return *them in the form of an array*.

**Example 1:**

```
Input: nums = [1,2,2,4]
Output: [2,3]

```

**Example 2:**

```
Input: nums = [1,1]
Output: [1,2]

```

**Constraints:**

- `2 <= nums.length <= 104`
- `1 <= nums[i] <= 104`

### Solution

`hashmap` 的解决思路
```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        auto res = vector<int>();
        int r_sum = nums.size() * (nums.size() + 1) / 2;
        auto maps = unordered_map<int, int>();

        for (int const& num : nums) {
            maps[num] += 1;

            if (maps[num] > 1) res.push_back(num);

            if (maps[num] == 1) {
                r_sum -= num;
            }
        }
        
        res.push_back(r_sum);

        return res;
    }
};
```