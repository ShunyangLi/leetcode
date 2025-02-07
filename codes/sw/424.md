# 424. Longest Repeating Character Replacement

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return *the length of the longest substring containing the same letter you can get after performing the above operations*.

 

**Example 1:**

```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
```

**Example 2:**

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.
```

## Solution

**核心思想**：

- 使用 **滑动窗口**（`left` 和 `right`）维护一个窗口，该窗口内的**最多 `k` 次替换**是否能让所有字符变成某个 `maxFreq` 最多的字符。
- 维护一个 `freq` 数组，记录窗口内每个字符的出现次数。
- 关键在于窗口大小 (`right - left + 1`) - `maxFreq` 是否 ≤ `k`：
  - 若满足，则窗口继续扩展 `right++`。
  - 若不满足，则收缩 `left++` 使窗口恢复合法性。

```c++
class Solution {
public:
    int max_repeat(unordered_map<char, int>& maps) {
        int max = 0;

        for (auto const& it : maps) {
            max = max > it.second ? max : it.second;
        }

        return max;
    }

    int characterReplacement(string s, int k) {
        auto maps = unordered_map<char, int>();
        int left = 0;
        int res = 0;


        for (int i = 0; i < s.size(); i ++) {
            char const c = s[i];

            if (maps.find(c) == maps.end()) maps[c] = 1;
            else maps[c] ++;

            while (i - left + 1 - max_repeat(maps) > k) {
                // invalid
                char const x = s[left];
                maps[x]--;
                left ++;
            }

            res = res < i - left + 1 ? i - left + 1 : res;

        }

        return res;
    }
};
```
