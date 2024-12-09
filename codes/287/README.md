# 287. Find the Duplicate Number

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return *this repeated number*.

You must solve the problem **without** modifying the array `nums` and using only constant extra space.

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2

```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3

```

**Example 3:**

```
Input: nums = [3,3,3,3,3]
Output: 3
```

**Constraints:**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?

### Solution

S1. 可以使用 `hashmap` 的思路来解决这个问题，解决方法比较简单，但是不满足 `O(1)` space的要求。

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        auto maps = unordered_map<int, int>();

        for (int const& num : nums) {
            maps[num] += 1;

            if (maps[num] > 1) return num;
        }

        return -1;
    }
};
```

S2. 使用快慢指针的方法来解决，因为题目中的要求其实已经声明了，数字不会超过数组长度，所以可以使用快慢指针的思路来找到cycle。

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = nums[0];
        int fast = nums[0];

        // Phase 1: Detect cycle
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        slow = nums[0];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow; // The duplicate number
    }
};
```