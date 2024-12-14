# 303. Range Sum Query - Immutable

Given an integer array `nums`, handle multiple queries of the following type:

1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

 

**Example 1:**

```
Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-105 <= nums[i] <= 105`
- `0 <= left <= right < nums.length`
- At most `104` calls will be made to `sumRange`.

## Solution

一开始想法是通过`vals[right] - vals[left]`的方式计算出来结果。但是我忽略了应该保留当前`val[left]`的值。所以直接`vals[right] - vals[left - 1]`就行。

**Note: 使用prefix解决**

```c++
class NumArray {
private:
    vector<int> vals;
public:
    NumArray(vector<int>& nums) {
        vals.resize(nums.size());

        vals[0] = nums[0];

        for (int i = 1; i < nums.size(); i ++) {
            vals[i] = vals[i - 1] + nums[i];
        }
    }
    
    int sumRange(int left, int right) {
        if (left == 0) return vals[right];
        return vals[right] - vals[left - 1];
    }
};
```



