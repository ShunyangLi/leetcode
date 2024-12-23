# 389. Find the Difference

You are given two strings `s` and `t`.

String `t` is generated by random shuffling string `s` and then add one more letter at a random position.

Return the letter that was added to `t`.

 

**Example 1:**

```
Input: s = "abcd", t = "abcde"
Output: "e"
Explanation: 'e' is the letter that was added.
```

**Example 2:**

```
Input: s = "", t = "y"
Output: "y"
```

 

**Constraints:**

- `0 <= s.length <= 1000`
- `t.length == s.length + 1`
- `s` and `t` consist of lowercase English letters.

## Solution

问就是`hashmap`。

```python
class Solution:
    def findTheDifference(self, s: str, t: str) -> str:
        maps1 = {}
        maps2 = {}

        for x in s:
            if x not in maps1:
                maps1[x] = 1
            else:
                maps1[x] += 1
        

        for x in t:
            if x not in maps1:
                return x
            
            if x not in maps2:
                maps2[x] = 1
            else:
                maps2[x] += 1
        

        for x in s:
            if maps1[x] != maps2[x]:
                return x
        
        return None
```

