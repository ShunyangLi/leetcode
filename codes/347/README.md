
# 347. Top K Frequent Elements

Given a non-empty array of integers, return the *k* most frequent elements.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

**Note:**

- You may assume *k* is always valid, 1 ≤ *k* ≤ number of unique elements.
- Your algorithm's time complexity **must be** better than O(*n* log *n*), where *n* is the array's size.
- It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
- You can return the answer in any order.

用HashMap可以解决，但是因为Java对HashMap有点点不太友善，我改成Python了，因为可以自定义sort 字典，这样可以根据结果排序了。。。

```python
 class Solution:
     def topKFrequent(self, nums, k):
         frequence = {}

         for value in nums:
             if value in frequence:
                 frequence[value] += 1
             else:
                 frequence[value] = 1

         frequence = sorted(frequence.items(), key=lambda x: x[1], reverse=True)
         index = 0
         res = []

         for t in frequence:
             index += 1
             res.append(t[0])
             if index == k:
                 break

         return res
```