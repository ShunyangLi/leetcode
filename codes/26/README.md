
# Remove Duplicates from Sorted Array

[Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)


Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

最获取长度的同时需要对array内部数据进行修改：

通过每次记录num，如果不相同的话，对数组的其实位置开始修改，然后curr是正在修改的index。（也会随着i增加而增加，如果所有数字都不一样）只不过是重新赋值了一次而已。

```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int dup = nums[0];
    int curr = 1;

    for (int i = 0; i < nums.length; i ++) {
        if (dup != nums[i]) {
            dup = nums[i];
            nums[curr++] = nums[i];
        }
    }

    return curr;
}
```