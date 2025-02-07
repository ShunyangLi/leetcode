# 395. Longest Substring with At Least K Repeating Characters

Given a string `s` and an integer `k`, return *the length of the longest substring of* `s` *such that the frequency of each character in this substring is greater than or equal to* `k`.

if no such substring exists, return 0.

 

**Example 1:**

```
Input: s = "aaabb", k = 3
Output: 3
Explanation: The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```
Input: s = "ababbc", k = 2
Output: 5
Explanation: The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only lowercase English letters.
- `1 <= k <= 105`

## Solution

**是的，单纯使用滑动窗口（不枚举 `targetUnique`）是无法直接解决的**，原因如下：

### **1. 传统滑动窗口的局限性**

普通滑动窗口通常用于：

- **固定窗口大小**（如 `k` 长度的子串）。
- **满足某个约束的最短/最长子串**（如 `>= k` 的某些条件）。

但在本题中：

- 你不知道窗口内应该包含多少种不同字符。
- 你无法确定哪些字符应该出现至少 `k` 次，哪些应该被排除。

### **解题思路**

1. **窗口左边界 `l` 和右边界 `r` 进行滑动**
   - `l`：窗口左端，逐步右移以收缩窗口。
   - `r`：窗口右端，逐步右移以扩展窗口。
2. **维护窗口内的字符频次**
   - 用 `freq[26]` 统计当前窗口内每个字符的频率。
3. **枚举窗口内的不同字符数量 `targetUnique`**
   - 题目要求 **至少有 `k` 个重复字符的最长子串**，但滑动窗口不能直接找到合适的 `k`，所以我们 **限制窗口内最多包含 `targetUnique` 种字符**，让窗口在符合 `k` 约束的前提下尽量扩展。
4. **滑动窗口扩展与收缩**
   - 先移动 `r` 扩展窗口，保证窗口内的字符数 ≤ `targetUnique`。
   - 如果窗口内字符数 > `targetUnique`，移动 `l` 收缩窗口。
   - 期间统计符合条件的最长子串。

```c++
class Solution {
public:
    int longestSubstring(string s, int k) {
        int res = 0;

        for (int target = 1; target <= 26; target ++) {
            auto frec = vector<int>(26, 0);
            int left = 0, valid_cnt = 0, unique = 0;

            for (int i = 0; i < s.size(); i ++) {
                int idx = s[i] - 'a';

                if (frec[idx] == 0) unique += 1;

                frec[idx] += 1;

                if (frec[idx] == k) valid_cnt += 1;

                // shrink
                while (unique > target) {
                    int l_idx = s[left] - 'a';

                    // < k can be multiple times
                    if (frec[l_idx] == k) valid_cnt --; 

                    frec[l_idx] --;
                    

                    if (frec[l_idx] == 0) unique --;

                    left ++;
                }

                if (unique == valid_cnt) res = max(res, i - left + 1);
            }
        }

        return res;
    }
};
```
