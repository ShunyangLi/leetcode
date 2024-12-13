# 227. Basic Calculator II

Given a string `s` which represents an expression, *evaluate this expression and return its value*. 

The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-231, 231 - 1]`.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

 

**Example 1:**

```
Input: s = "3+2*2"
Output: 7
```

**Example 2:**

```
Input: s = " 3/2 "
Output: 1
```

**Example 3:**

```
Input: s = " 3+5 / 2 "
Output: 5
```

 ## Solution

做一个优先级运算就行。先算`*`和`/`。

### Baseline

```c++
class Solution {
public:
    int calculate(string s) {
        auto vals = vector<int>();

        string tmp = "";
        char last = ' ';

        for (char const& x : s) {
            if (x == ' ') continue;

            if (isdigit(x)) {
                tmp += x;
            } else {

                if (last == '-') {
                    vals.push_back( 0 - stoi(tmp));
                } else {
                    vals.push_back(stoi(tmp));
                }

                if (last == '*' || last == '/') {
                    int idx = vals.size() - 1;

                    int val = last == '*' ? vals[idx] * vals[idx - 1] : vals[idx-1] / vals[idx];

                    vals.pop_back();
                    vals.pop_back();

                    vals.push_back(val);
                }

                tmp = "";
                last = x;
            }
        }

        if (last == '-') {
            vals.push_back( 0 - stoi(tmp));
        } else {
            vals.push_back(stoi(tmp));
        }

        if (last == '*' || last == '/') {
            int idx = vals.size() - 1;

            int val = last == '*' ? vals[idx] * vals[idx - 1] : vals[idx-1] / vals[idx];

            vals.pop_back();
            vals.pop_back();

            vals.push_back(val);
        }

        int res = 0;

        for (int const & val : vals) res += val;

        return res;


    }
};
```

### Adv

```c++
class Solution {
public:
    int calculate(string s) {
        vector<int> vals;
        int num = 0;
        char lastOp = '+';

        for (size_t i = 0; i < s.size(); ++i) {
            char c = s[i];

            if (isdigit(c)) {
                num = num * 10 + (c - '0');
            }

            if (!isdigit(c) && c != ' ' || i == s.size() - 1) {
                if (lastOp == '+') {
                    vals.push_back(num);
                } else if (lastOp == '-') {
                    vals.push_back(-num);
                } else if (lastOp == '*' || lastOp == '/') {
                    int prev = vals.back();
                    vals.pop_back();
                    vals.push_back(lastOp == '*' ? prev * num : prev / num);
                }

                lastOp = c;
                num = 0;
            }
        }

        return accumulate(vals.begin(), vals.end(), 0);
    }
};
```

