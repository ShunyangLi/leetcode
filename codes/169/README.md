
# Majority Element

[Majority Element](https://leetcode.com/problems/majority-element/)

Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

**Example 1:**

```
Input: [3,2,3]
Output: 3
```

**Example 2:**

```
Input: [2,2,1,1,1,2,2]
Output: 2
```

看到这一类的题，最先想到的就是使用HashMap来解决这个计数的问题，当计数的结果`> nums.leng/2`的时候可以直接return就好了，准没错

```java
public int majorityElement(int[] nums) {
    if (nums.length <= 0) return -1;
    int len = nums.length - 1;
    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i <= len; i ++) {
        if (map.containsKey(nums[i])) {
            if (map.get(nums[i]) + 1 >= nums.length/2) return nums[i];
            map.put(nums[i], map.get(nums[i]) + 1);
        } else {
            map.put(nums[i], 1);
        }
    }

    return -1;
}
```

然后看了一下官方的解法，就是先sort一下，如果该数字的个数大于总array长度的一般也就意味着sort完之后取中间那个数字准没错。。。那么简单的方法咋就没想到呢o(╥﹏╥)o

```java
public int majorityElement(int[] nums) {
    if (nums.length == 0) return -1;
    Arrays.sort(nums);
    return nums[nums.length/2];
}
```