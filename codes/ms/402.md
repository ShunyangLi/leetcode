# 402. Remove K Digits

Given string num representing a non-negative integer `num`, and an integer `k`, return *the smallest possible integer after removing* `k` *digits from* `num`.

**Example 1:**

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

```

**Example 2:**

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

```

**Example 3:**

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.

```

## Solution

用`stack`维护一个递减的序列，如果当前的数字比`stack`里面的元素小，则把当前元素替换成`stack`里面的元素。可以用`stack`或者`deque`都行。

```c
class Solution {
public:
    string removeKdigits(string num, int k) {
        if (num.size() == k) return "0";

        stack<char> s;

        for (char c : num) {
            while (!s.empty() && k > 0 && c < s.top()) {
                s.pop();
                k--;
            }
            s.push(c);
        }

        while (k > 0 && !s.empty()) {
            s.pop();
            k--;
        }

        string res = "";
        while (!s.empty()) {
            res += s.top();
            s.pop();
        }
        reverse(res.begin(), res.end());

        int i = 0;
        while (i < res.size() && res[i] == '0') i++;

        res = res.substr(i);
        return res.empty() ? "0" : res;
    }
};

```

`deque`代码：

```c
class Solution {
public:
    string removeKdigits(string num, int k) {
        if (num.size() == k) return "0";

        deque<char> stack;

        for (char const& c : num) {
            while (!stack.empty() && k > 0 && c < stack.back()) {
                stack.pop_back();
                k--;
            }
            stack.push_back(c);
        }

        while (k > 0 && !stack.empty()) {
            stack.pop_back();
            k--;
        }

        string res = "";
        for (char const& c : stack) {
            if (res.empty() && c == '0') continue;
            res += c;
        }

        return res.empty() ? "0" : res;
    }
};

```
