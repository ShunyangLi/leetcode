
# Intersection of Two Arrays II

[Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

Given two arrays, write a function to compute their intersection.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```

**Note:**

- Each element in the result should appear as many times as it shows in both arrays.
- The result can be in any order.

这个题就稍微简单一点了，可以用HashMap来记录`num1`的元素，value来存数字出现的次数。这样在遍历`num2`的时候就可以直接判断了。。。刚开始想到过array的用法，但是因为key是单一的，默默被我放弃了（我就是傻逼。。。），看了一下discussion恍然大悟。。。

```java
import javax.swing.*;
import java.util.*;

/**
 * Input: nums1 = [1,2,2,1], nums2 = [2,2]
 * Output: [2,2]
 * 没啥思路， 傻逼了，想着可以用hashmap但是，想到hashmap的key只能存一个value
 * 但是value可以表示数量啊
 * 傻逼
 *
 */
public class Intersection {
    public int[] intersect(int[] nums1, int[] nums2) {
        HashMap<Integer, Integer> maps = new HashMap<>();
        ArrayList<Integer> res = new ArrayList<Integer>();

        for (Integer num : nums1) {
            if (maps.containsKey(num)) {
                maps.put(num, maps.get(num) + 1);
            } else {
                maps.put(num, 1);
            }
        }

        for (Integer num : nums2) {
            if (maps.containsKey(num)) {
                res.add(num);
                if (maps.get(num) == 1) maps.remove(num);
                else maps.put(num, maps.get(num) - 1);
            }
        }
        int[] result = new int[res.size()];
        for (int i = 0; i < res.size(); i ++) result[i] = res.get(i);
        return result;
    }

    public static void main(String[] args) {
        int[] num1 = new int[] {1,2,2,1};
        int[] num2 = new int[] {2,2};
        Intersection i = new Intersection();
        int[] res = i.intersect(num1, num2);
        System.out.println(Arrays.toString(res));
    }
}
```

