
# 39. Combination Sum

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

这个可以使用到回溯算法的思想，从`i - nums.length`，每个都会重复，比如当`i=0`的时候：

```
2,2,2,2
2,2,2,3
2,2,2,6
2,2,2,7
2,2,3
2,2,6
2,2,7
2,2
2,3
....
```

这样的一个规律。

```java
import java.util.*;

public class CombinationSum {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new LinkedList<>();
        back_tracking(res, new ArrayList<>(), target, candidates, 0);
        return res;
    }

    // 回溯算法
    public void back_tracking(List<List<Integer>> res, List<Integer> temp, int target, int[] nums, int index) {
        if (target < 0) return;
        else if (target == 0) res.add(new ArrayList<>(temp));
        else {
            for (int i = index; i < nums.length; i ++) {
                temp.add(nums[i]);
                back_tracking(res, temp, target - nums[i], nums, i);
                temp.remove(temp.size() - 1);
            }
        }
    }

    public static void main(String[] args) {
        CombinationSum cs = new CombinationSum();
        int[] nums = new int[] {2,3,6,7};
        List<List<Integer>> res = cs.combinationSum(nums, 7);

        for (List<Integer> l : res) {
            System.out.println(Arrays.toString(l.toArray()));
        }
    }
}
```