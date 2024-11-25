
# 216. Combination Sum III
[Combination Sum III](https://leetcode.com/problems/combination-sum-iii/): Find all valid combinations of k numbers that sum up to n such that the following conditions are true:

- Only numbers 1 through 9 are used.
- Each number is used at most once.


Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order. For example:
```
Input: k = 3, n = 7
Output: [[1,2,4]]
Explanation:
1 + 2 + 4 = 7
There are no other valid combinations.
```
This question is pretty similar with the previous one, just add one more constrain (sum = n).
```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        def combine(k, n, index, path, result):
            if len(path) == k:
                if sum(path) == n:
                    result.append([i for i in path])
                return path

            for i in range(index, 10):
                path.append(i)
                combine(k, n, i + 1, path, result)

                path.pop()

            return result
    
        return combine(k, n, 1, [], [])
```
