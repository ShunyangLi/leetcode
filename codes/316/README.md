# 316. Remove Duplicate Letters

Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is

**the smallest in lexicographical order**

among all possible results.

**Example 1:**

```
Input: s = "bcabc"
Output: "abc"

```

**Example 2:**

```
Input: s = "cbacdcbc"
Output: "acdb"

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.

**Note:** This question is the same as 1081: [https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

## Solution

这个题还是通过使用`stack`来实现的，但是值得注意的就是字母在pop的时候看看是不是最后一次出现，然后判断一下该字母是否已经存在`stack`里面。

```c
class Solution {
public:
    string removeDuplicateLetters(string s) {
        deque<char> stack;

        auto nums = vector<int>(26, 0);
        auto in_stack = vector<bool>(26, false);

        string res = "";

        for (int i = 0; i < s.size(); i ++) nums[s[i] - 'a']++;

        for (char const & c : s) {

            nums[c - 'a'] --;

            if (in_stack[c - 'a']) continue;

            while (!stack.empty() && stack.back() > c && nums[stack.back() - 'a'] >= 1) {

                in_stack[stack.back() - 'a'] = false;

                stack.pop_back();
            }

            stack.push_back(c);
            in_stack[c - 'a'] = true;
        }

        while (!stack.empty()) {
            res += stack.front();
            stack.pop_front();
        }

        return res;

    }
};

```