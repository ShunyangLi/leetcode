
# 162. Find Peak Element

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

**Follow up:** Your solution should be in logarithmic complexity.

可以采用一个二分查找的方法来处理这个问题，因为已知peak number是 `nums[i] > nums[i+1]`也就是说如果在array里面出现一个降序就代表着是peak number。这样的话可以使用二分查找，如果`nums[mid] > nums[mid+1]`就代表该数字就是peak number，但是之前的可以还有相对应的peak number，所以就是`search(nums, left, mid)`。

ps: 也有一种O(n)的方法来查找。

```java
public class FindPeakElement {
//    public int findPeakElement(int[] nums) {
//        for (int i = 0; i < nums.length - 1; i ++) {
//            if (nums[i] > nums[i+1]) return i;
//        }
//
//        return nums.length-1;
//    }

    public int findPeakElement(int[] nums) {
        return search(nums, 0, nums.length - 1);
    }

    public int search(int[] nums, int left, int right) {
        if (left == right) return left;

        int mid = (left + right) / 2;
        if (nums[mid] > nums[mid+1]) return search(nums, left, mid);

        return search(nums, mid+1, right);
    }
}

```