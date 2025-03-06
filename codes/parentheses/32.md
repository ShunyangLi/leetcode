# 32. Longest Valid Parentheses

Given a string containing just the characters `'('` and `')'`, return *the length of the longest valid (well-formed) parentheses* *substring*.

**Example 1:**

```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```

**Example 2:**

```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
```

**Example 3:**

```
Input: s = ""
Output: 0
```

## Solution

使用`stack`可以解决这个问题，需要注意的就是我们需要在栈底记录一下第一个**invalid** 的位置。这样的话我只需要用当前位置的index减去`stack`里面的就能拿到最大长度。

Note：一般找长度的话`stack`记录index会比较合适

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> sk;

        int res = 0;
        sk.push(-1);

        for (int i = 0; i < s.size(); i ++) {
            if (s[i] == '(') {
                sk.push(i);
            } else {
                sk.pop();

                if (sk.empty()) {
                    sk.push(i);
                } else {
                    res = max(res, i - sk.top() );
                }
            }A
        }

        return res;
    }
};
```

**Example:**

```
初始:       栈: [-1]
)  pop (空栈)   => 栈: [0]      // 无法匹配，存当前索引
(  push        => 栈: [0, 1]
)  pop (匹配)   => 栈: [0]      // 匹配 "()" 计算长度: 2
(  push        => 栈: [0, 3]
)  pop (匹配)   => 栈: [0]      // 匹配 "()" 计算长度: 4
)  pop (空栈)   => 栈: [5]      // 无法匹配，存当前索引
```

初始化存的不符合规定的是-1，遇到`)`，发现不match，把之前的pop出去，存入当前的index。