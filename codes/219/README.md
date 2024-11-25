
# 219. Contains Duplicate II

Given an array of integers and an integer *k*, find out whether there are two distinct indices *i* and *j* in the array such that **nums[i] = nums[j]** and the **absolute** difference between *i* and *j* is at most *k*.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

**Example 3:**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

本来想的是直接用for循环，根据当前数字然后向后走k位，看k位以内有没有和`nums[i]`相同的数字，但是貌似bug有点多，放弃了。。改用HashMap了，key存`nums[i]`，value存相对应的index。如果HashMap已经有该key则可以计算HashMap里面存的index和当前i的差值，如果小于等于K就可以直接返回了。

```java
import java.util.HashMap;
import java.util.Map;

public class ContainsDuplicateII {
    // 放弃了我使用hashmap来写好吧。。。
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i ++) {
            if (map.containsKey(nums[i])) {
                if (i - map.get(nums[i]) <= k) return true;
            }

            map.put(nums[i], i);
        }

        return false;
    }

    public static void main(String[] args) {
        ContainsDuplicateII c = new ContainsDuplicateII();
        int[] nums = new int[] {1,2,3,1,2,3};

        System.out.println(c.containsNearbyDuplicate(nums, 2));
    }
}
```