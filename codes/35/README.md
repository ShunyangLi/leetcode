
# Search insert position

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

就是找到合适的位置，然后返回相对应的index，没什么难度：

```java
public int searchInsert(int[] nums, int target) {

    for (int i = 0; i < nums.length; i ++) {
        if (nums[i] >= target) return i;
    }

    return nums.length;
}
```
