# 75. Sort Colors

Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

 

**Example 1:**

```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Example 2:**

```
Input: nums = [2,0,1]
Output: [0,1,2]
```

 

**Constraints:**

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either `0`, `1`, or `2`.

 

**Follow up:** Could you come up with a one-pass algorithm using only constant extra space?

### Solution

可以使用`hashmap`或者直接count数量也行。

### S1

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        auto maps = unordered_map<int, vector<int>>();

        for (int const& num : nums) {
            maps[num].push_back(num);
        }

        int idx = 0;

        for (int i = 0; i <= 2; i ++) {
            if (maps.find(i) == maps.end()) continue;

            for (int const& val : maps[i]) {
                nums[idx++] = val;
            }
        }
    }
};
```

### S2

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int cnt0 = 0;
        int cnt1 = 0;
        int cnt2 = 0;

        for (int const & num : nums) {
            if (num == 0) cnt0 ++;
            if (num == 1) cnt1 ++;
            if (num == 2) cnt2 ++;
        }

        int idx = 0;
        
        for (int i = 0; i < cnt0; i ++) nums[idx++] = 0;
        for (int i = 0; i < cnt1; i ++) nums[idx++] = 1;
        for (int i = 0; i < cnt2; i ++) nums[idx++] = 2;
    }
};
```



