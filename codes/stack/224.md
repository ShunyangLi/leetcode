# 224. Basic Calculator

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return *the result of the evaluation*.

**Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

 

**Example 1:**

```
Input: s = "1 + 1"
Output: 2
```

**Example 2:**

```
Input: s = " 2-1 + 2 "
Output: 3
```

**Example 3:**

```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

 

**Constraints:**

- `1 <= s.length <= 3 * 105`
- `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
- `s` represents a valid expression.
- `'+'` is **not** used as a unary operation (i.e., `"+1"` and `"+(2 + 3)"` is invalid).
- `'-'` could be used as a unary operation (i.e., `"-1"` and `"-(2 + 3)"` is valid).
- There will be no two consecutive operators in the input.
- Every number and running calculation will fit in a signed 32-bit integer.

## Solution

用`stack`来解决，核心在于每次运算括号里面的数据，在运算括号里内容的时候先把之前的数据都放进stack里面，计算完括号里的内容之后把之前的运算结果拿出来进行计算。有点像递归的思想。想到用`stack`了，但是算法思路没那么细致，需要多写

```c++
class Solution {
public:
    int calculate(string s) {

        auto stack = std::stack<int>();
        int op = 1;
        int val = 0;

        int num = 0;

        for (char const& x : s) {

            if (x == ' ') continue;

            if (isdigit(x)) {
                num = num * 10 + (x - '0');
            } else if (x == '+') {
                val += op * num;
                op = 1;
                num = 0;
            } else if (x == '-') {
                val += op * num;
                op = -1;
                num = 0;
            } else if (x == '(') {
                stack.push(val);
                stack.push(op);
                val = 0;
                op = 1;
                num = 0;
            } else if (x == ')') {
                val += op * num;
                val *= stack.top();
                stack.pop();
                val += stack.top();
                stack.pop();
                num = 0;
            }
        }

        val += op * num;

        return val;
    }
};
```

