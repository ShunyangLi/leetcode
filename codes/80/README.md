# 80. Remove Duplicates from Sorted Array II

Given an integer array `nums` sorted in **non-decreasing order**, remove some duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears **at most twice**. The **relative order** of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

Do **not** allocate extra space for another array. You must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3]
Output: 5, nums = [1,1,2,2,3,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2:**

```
Input: nums = [0,0,1,1,1,1,2,3,3]
Output: 7, nums = [0,0,1,1,2,3,3,_,_]
Explanation: Your function should return k = 7, with the first seven elements of nums being 0, 0, 1, 1, 2, 3 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

 ## Solution

一开始理解错了，以为只要返回数量就行，后面发现还需要额外的修改内存数据。其实这个题很简单，只需要用到双指针就行，或者快慢指针的方法，意思都是差不多的。`idx`指针负责去遍历`nums`数据，`s_idx`负责记录更新开始的坐标。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int idx = 0, s_idx = 0, val = 0, cnt = 0;

        while (idx < nums.size()) {
            val = nums[idx];
            cnt = 0;
            
            int j = idx;
            while (j < nums.size() && nums[j] == val) {
                cnt ++;
                if (cnt <= 2) nums[s_idx++] = nums[j];       
                j ++;
            }

            idx = j;
        }

        return s_idx;
    }
};
```

