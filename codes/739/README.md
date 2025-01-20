# **739. Daily Temperatures**

Given an array of integers `temperatures` represents the daily temperatures, return *an array* `answer` *such that* `answer[i]` *is the number of days you have to wait after the* `ith` *day to get a warmer temperature*. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

**Example 1:**

```
Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]

```

**Example 2:**

```
Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]

```

**Example 3:**

```
Input: temperatures = [30,60,90]
Output: [1,1,0]

```

**Constraints:**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

## Solution


```
Temperature: [73, 74, 75, 71, 69, 72, 76, 73]
stack = []
res = [0, 0, 0, 0, 0, 0, 0, 0]

1. index = 0
stack = [(0, 73)]

2. index = 1
74 > 73, pop (0, 73) and set res[0] = index - 0
stack = [(1, 74)]
res = [1, 0, 0, 0, 0, 0, 0, 0]

3. index = 2
75 > 74, pop (1, 74) and set res[1] = index - 1
stack = [(2, 75)]
res = [1, 1, 0, 0, 0, 0, 0, 0]

4. index = 3
71 < 75, add (3, 71) into stack
stack = [(2, 75), (3, 71)]

5. index = 4
69 < 71, add (3, 69) into stack
stack = [(2, 75), (3, 71), (4, 69)]

6. index = 5
72 < 69, pop (4, 69) and (3, 71), set res[4] = index - 4, and res[3] = index - 3
stack = [(2, 75), (5, 72)]
res = [1, 1, 0, 2, 1, 0, 0, 0]

7. index = 6
76 > 72 pop (5, 72) and (2, 75), set res[5] = index - 5, and res[2] = index -2
stack = []
res = [1, 1, 4, 2, 1, 1, 0, 0]
```

这个是真是面试题，21年的时候面试的时候遇到了。可以采用 `stack` 数据结构的解决方法。当遍历当前温度的时候去判断 `stack` 里面的元素是否小于当前温度，如果小于的话则 `pop` ，最后结束循环的时候把当前温度添加到 `stack` 中，因为每个温度都要找到比当前大的温度。
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        auto stack = std::vector<pair<int, int>>();
        auto res = vector<int>(temperatures.size(), 0);


        for (int i = 0; i < temperatures.size(); i ++) {
            int t = temperatures[i];
            if (stack.size() == 0) {
                stack.push_back(pair(t, i));
                continue;
            }

            while (!stack.empty()) {
                pair<int, int> val = stack.back();

                if (val.first >= t) break;

                res[val.second] = i - val.second;
                stack.pop_back();
            }

            stack.push_back(pair(t, i));

        }
        return res;
    }
};
```


每次都有不同的想法，但是思路和代码越来越简单高效，还是用`stack`

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> stack;
        auto res = vector<int>(temperatures.size(), 0);

        for (int i = 0; i < temperatures.size(); i ++) {
            
            while (!stack.empty() && temperatures[stack.top()] < temperatures[i]) {
                int idx = stack.top();

                res[idx] = i - idx;

                stack.pop();
            }

            stack.emplace(i);
        }

        return res;
    }
};
```