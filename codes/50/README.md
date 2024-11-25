# Pow(x, n)

[ Pow(x, n)](https://leetcode.com/problems/powx-n/)

Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* (xn).

**Example 1:**

```
Input: 2.00000, 10
Output: 1024.00000
```

**Example 2:**

```
Input: 2.10000, 3
Output: 9.26100
```

**Example 3:**

```
Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

**Note:**

- -100.0 < *x* < 100.0
- *n* is a 32-bit signed integer, within the range [−231, 231 − 1]

这个就是简单的求一下平方，然后给保留5位小数就行了，用Python简单的写了一波

```python
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        x = x**n
        
        x = 2**32-1 if x >= 2**32-1 else x
        x = -2**32 if x <= -2**32 else x
        
        return round(x, 5)
        
```