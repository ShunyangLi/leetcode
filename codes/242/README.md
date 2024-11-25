
# 242. Valid Anagram
Given two strings `s` and `t`, return true if `t` is an anagram of `s`, and false otherwise.

```
Input: s = "anagram", t = "nagaram"
Output: true
Input: s = "rat", t = "car"
Output: false
```

## Brute Force
This issue can be solved by using two loop. But the time complexity is $O(N)$. In addition, it also can be done through sort algorithm, to check whether same. But it is too complex. Here we did not code for this approache.

## Advance Approach
We can use `hashmap` to record that how many times they appeared in the string. Therefore, the time complexity is $O(N)$.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        
        data = {}

        for i in s:
            if i not in data:
                data[i] = 1
            else:
                data[i] += 1

        for i in t:
            if i not in data:
                return False
            else:
                if data[i] == 0:
                    return False
                else:
                    data[i] -= 1

        for key, val in data.items():
            if data[key] != 0:
                return False

        return True
```