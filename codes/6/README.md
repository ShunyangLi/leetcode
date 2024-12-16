# 6. Zigzag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);
```

 

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```

**Example 3:**

```
Input: s = "A", numRows = 1
Output: "A"
```

 

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists of English letters (lower-case and upper-case), `','` and `'.'`.
- `1 <= numRows <= 1000`

## Solution

以前做不出来的题现在好像都很容易？？？其实只要找到规律就行，直到斜着的char的数量是`numRows - 2`就行，然后收集到数组，最后运算就行。不过也可以直接计算出来最后的string，但是比较麻烦，不是很想折腾。

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        auto maps = vector<vector<char>>(numRows);
        string res = "";

        int idx = 0;
        bool add_row = true;

        while (idx < s.size()) {
            if (add_row) {
                int cnt = 0;
                while (idx < s.size() && cnt < numRows) {
                    maps[cnt ++].push_back(s[idx]);
                    idx ++;
                }
                add_row = false;
            } else {
                int cnt = numRows - 2;
                while (idx < s.size() && cnt > 0) {
                    maps[cnt --].push_back(s[idx]);
                    idx ++;
                }

                add_row = true;
            }
        }

        for (auto & map : maps) {
            for (char const& x : map) res += x;
        }

        return res;
    }
};
```

