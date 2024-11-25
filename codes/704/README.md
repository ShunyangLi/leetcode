
# 704. Binary Search

Given a **sorted** (in ascending order) integer array `nums` of `n` elements and a `target` value, write a function to search `target` in `nums`. If `target` exists, then return its index, otherwise return `-1`.


**Example 1:**

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

**Example 2:**

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

标准的二分查找法（只能用在排过序的list里面）。有一个算法讲解：

<iframe width="807" height="605" src="https://www.youtube.com/embed/4S5dSTNYafU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```java
public class BinarySearch {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] > target) right = mid - 1;
            else left = mid + 1;
        }

        return -1;
    }

    public static void main(String[] args) {
        int[] nums = new int[] {-1,0,3,5,9,12};
        BinarySearch bs = new BinarySearch();
        int res = bs.search(nums, 9);
        System.out.println(res);
    }
}
```
