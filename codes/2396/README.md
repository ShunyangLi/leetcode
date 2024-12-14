# 2396. Strictly Palindromic Number

An integer `n` is **strictly palindromic** if, for **every** base `b` between `2` and `n - 2` (**inclusive**), the string representation of the integer `n` in base `b` is **palindromic**.

Given an integer `n`, return `true` *if* `n` *is **strictly palindromic** and* `false` *otherwise*.

A string is **palindromic** if it reads the same forward and backward.

 

**Example 1:**

```
Input: n = 9
Output: false
Explanation: In base 2: 9 = 1001 (base 2), which is palindromic.
In base 3: 9 = 100 (base 3), which is not palindromic.
Therefore, 9 is not strictly palindromic so we return false.
Note that in bases 4, 5, 6, and 7, n = 9 is also not palindromic.
```

**Example 2:**

```
Input: n = 4
Output: false
Explanation: We only consider base 2: 4 = 100 (base 2), which is not palindromic.
Therefore, we return false.
```

 

**Constraints:**

- `4 <= n <= 105`

## Solution

遍历一下然后用双指针就行

```c++
class Solution {
public:
    bool isStrictlyPalindromic(int n) {
        if (n == 0) return false;

        for (int b = 2; b <= n -2; b ++) {
            auto vals = vector<int>();
            int x = n;

            while (x > 0) {
                vals.push_back(x % b);
                x /= b;
            }

            int s = 0, e = vals.size() - 1;

            while (s < e) {
                if (vals[s] != vals[e]) return false;
                s++;
                e--;
            }
        }

        return true;
    }
};
```

