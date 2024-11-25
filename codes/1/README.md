
# Two Sum

[Two Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

这也是我处女面试的第一个算法，当时想的方法是最暴力的方法，直接两个循环完事，但是时间复杂度是$O(n^{2})$，那是相当的高o(╥﹏╥)o。后来想了一下找到了一个简单的方法，当时由于太紧张写的不太好。当时的问题方法也稍微有点不同，*当时要求的是找到相对应的数字，不是index*。 先回顾一下当时的算法：

```java
int[] another(int[] nums, int target) {
    int head = 0;
    int tail = 0;

    // sort it
    Arrays.sort(nums);

    while (true) {
        int value = nums[head] + nums[tail];

        if (value == target) break;
        else if (value > target) tail -= 1;
        else head += 1;

        if (head == tail) break;
    }

    if (head != tail) return new int[] {nums[head], nums[tail]};
    return null;
}
```

当时想到的是两头同时遍历的方法，但是有个前提要求就是list必须是排过序的，当`nums`无限大时排序所消耗的时间可以忽略的，这个方法有点类似于**binary search**。我当时也想过用`HashMap`，好像处于什么原因被面试官否定了，whatever不重要的。不闲扯了，我们开始看这题的比较优化的算法，时间复杂度是`O(n)`。

```java
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums.length; i ++) {
        int minus = target - nums[i];
        if (map.containsKey(minus)) {
            return new int[]{map.get(minus), i};
        } else {
            map.put(nums[i], i);
        }
    }

    return null;
}
```

算法思路：

1. 因为是用的`HashMap`所以在检测key是否contain的话使用的是hash的方法，所以复杂度是$O(1)$
2. 第7-11行，是来判断这个HashMap是否包含这个num，如果这个num不包含在HashMap里面，然后把该num存到HashMap里面，num当做key，index当做value。这样就相当于把访问过的num和num的index存到HashMap里面，然后再遍历nums的时候得对每个数可以得到一个差值，然后判断这个差值是否存在HashMap里面，如果存在就代表得到这个结果了，如果不存在就把这个`nums[i]`存到HashMap。
