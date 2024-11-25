
# Contains Duplicate

[Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

**Example 1:**

```
Input: [1,2,3,1]
Output: true
```

**Example 2:**

```
Input: [1,2,3,4]
Output: false
```

**Example 3:**

```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```

这个题用简单的HashMap做就行，或者用hashset，我这用的是hashset。

```java
import java.util.Arrays;
import java.util.HashSet;

public class ContainsDuplicate {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> hashSet = new HashSet<Integer>();

        for (int num : nums) {
            hashSet.add(num);
        }

        return nums.length != hashSet.size();
    }

    public static void main(String[] args) {
        int[] nums = new int[]{1,2,3, 1};
        ContainsDuplicate cd = new ContainsDuplicate();
        System.out.println(cd.containsDuplicate(nums));
    }
}
```
