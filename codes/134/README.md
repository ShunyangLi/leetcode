# 134. Gas Station

There are `n` gas stations along a circular route, where the amount of gas at the `ith` station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return *the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return* `-1`. If there exists a solution, it is **guaranteed** to be **unique**.

 

**Example 1:**

```
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

**Example 2:**

```
Input: gas = [2,3,4], cost = [3,4,3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

 

**Constraints:**

- `n == gas.length == cost.length`
- `1 <= n <= 105`
- `0 <= gas[i], cost[i] <= 104`

## Solution

### Baseline

一开始没很清楚的理解题意，选了一个最复杂的解法 O(n^2)吧。

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        if (gas.size() != cost.size()) return -1;

        for (int i = 0; i < gas.size(); i ++) {
            int ful = gas[i];

            int j = i + 1;

            while (j % gas.size() != i) {
                ful -= cost[(j - 1) % gas.size()];
                
                if (ful < 0) break;
                ful += gas[j % gas.size()];

                j++;
            }

            ful -= cost[(j - 1) % gas.size()];

            if (ful >= 0) return i;
        }

        return -1;
    }
};
```

### Greedy

**逻辑核心**：

- 和 Python 实现类似，使用两个变量 `total_tank` 和 `curr_tank` 分别记录总剩余油量和当前路径的剩余油量。
- 当 `curr_tank` 小于 0 时，说明以当前起始点无法完成路径，将起始点更新为下一个加油站。

**复杂度分析**：

- **时间复杂度**：O(n)，只需遍历加油站一次。
- **空间复杂度**：O(1)，只使用了常量空间。

**函数调用**：

- 传入两个向量 `gas` 和 `cost`，分别表示每个加油站的汽油量和到下一个加油站的消耗量。
- 返回值是一个整数，表示起始加油站的索引；若无解则返回 `-1`。

### 理解题目提示

可能产生疑惑的地方在于题目描述的环形路径性质：

1. 环形路径意味着从最后一个加油站的消耗是回到第一个加油站，这种性质可能让人误以为需要用 `cost[i-1]` 之类的索引操作。
2. 实际上，`gas[i] - cost[i]` 的直接计算已经涵盖了加油和消耗的本质，题目所说的环形只影响索引在逻辑上是循环的。

### 重点：环形路径的核心

环形路径的特性只是加油站编号的逻辑循环，它在编程中直接体现在索引运算的边界处理上：

- `i = n - 1` 到 `i = 0` 这种跳跃是循环路径的体现。
- 数组中并没有实际的额外复杂性，直接计算即可。

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total_tank = 0;
        int curr_tank = 0;
        int start_station = 0;

        for (int i = 0; i < gas.size(); ++i) {
            total_tank += gas[i] - cost[i];
            curr_tank += gas[i] - cost[i];
            
            if (curr_tank < 0) {
                start_station = i + 1;
                curr_tank = 0;
            }
        }
        return total_tank >= 0 ? start_station : -1;
    }
};
```



