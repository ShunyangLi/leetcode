# 41. First Missing Positive

Given an unsorted integer array `nums`. Return the *smallest positive integer* that is *not present* in `nums`.

You must implement an algorithm that runs in `O(n)` time and uses `O(1)` auxiliary space.

 

**Example 1:**

```
Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.
```

**Example 2:**

```
Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.
```

**Example 3:**

```
Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`

## Solution

1. **核心思想**：
   - 使用输入数组本身作为哈希表，重新排列数组，使得数字`x`尽可能放到索引`x-1`的位置 （因为最小正整数是1）。
   - 最后一次遍历，找到第一个位置 `i` 满足 `nums[i]≠i+1`，则 `i+1` 即为第一个缺失的正整数。
2. **算法步骤**：
   1. 遍历数组，对于每个元素`nums[i]`
      1. 如果`nums[i]`在`1`到`n`范围内，并且`nums[nums[i]−1]≠nums[i]`，将`nums[i]`放到正确的位置（即交换到索引`nums[i]−1`）。
   2. 遍历排序后的数组，找到第一个`nums[i]≠i+1`的位置，返回`i+1`。
   3. 如果数组所有位置都满足`nums[i]=i+1`，返回`n+1`。

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        
        for (int i = 0; i < nums.size(); i ++) {

           while (true) {
                if (nums[i] >= 1 && nums[i] - 1 < nums.size() && nums[nums[i] - 1] != nums[i]) {
                    int tmp = nums[nums[i] - 1];
                    nums[nums[i] - 1] = nums[i];
                    nums[i] = tmp;

                    continue;
                }

                break;
            }
        }

        int i = 0;
        for (; i < nums.size(); i ++) {
            if (nums[i] != i + 1) return i + 1;
        }

        return i + 1;
    }
};
```

