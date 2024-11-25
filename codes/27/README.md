
# Remove Element

[Remove Element](https://leetcode.com/problems/remove-element/)

Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

这个和前面那个remove duplicate number是一个思路：

这个算法很简单，但是效率贼高。其实很好理解，就是当这个数字不等于val的时候，会用之前的curr（非重复的数字坐标）来替换当前的val。

```
3, 2, 2, 3 & val = 2
curr = 0

when i = 0, val != nums[i] num[curr++] = nums[i] -> nums[0] = nums[0] and curr = 1 now
when i = 1, val == nums[i] continue
when i = 2, val == nums[i] continue
when i = 3, val != nums[i] num[curr++] = nums[i] -> nums[1] = nums[3] and curr = 2 now

so nums = 3, 3, 2, 3 and curr = 2, we do not care index > 1 values,
so we done
```

```java
public int removeElement(int[] nums, int val) {
    int curr = 0;

    for (int i = 0; i < nums.length; i++) {
        if (val != nums[i]) {
            nums[curr++] = nums[i];
        }
    }

    return curr;
}
```
