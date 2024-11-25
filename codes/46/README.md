# 46. Permutations

Given a collection of **distinct** integers, return all possible permutations.

**Example:**

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

也是一个回溯算法，需要加一个额外的条件，不能重复，不过也是回溯算法的一个套路。伪代码：

```python
result = []
def backtrack(path, select_list):
    if condition:
        result.add(path)
        return

    for list in select_list:
        make a select
        backtrack(path, select_list)
        drawback select
```

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;


public class Permutations {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new LinkedList<>();

        back_track(res, new ArrayList<>(), nums);
        return res;
    }


    public void back_track(List<List<Integer>> res, List<Integer> temp, int[] nums) {
        if (temp.size() == nums.length) {
            res.add(new ArrayList<>(temp));
            return;
        }

        for (int i = 0; i < nums.length; i ++) {
            // 避免重复项
            if (temp.contains(nums[i])) continue;
            temp.add(nums[i]);
            back_track(res, temp, nums);
            temp.remove(temp.size() - 1);
        }
    }

    public static void main(String[] args) {
        int[] nums = new int[] {1,2,3};

        Permutations p = new Permutations();
        List<List<Integer>> res = p.permute(nums);

        for (List<Integer> l : res) {
            System.out.println(Arrays.toString(l.toArray()));
        }
    }
}
```
