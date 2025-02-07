# Greedy algorithm - 背包问题

## 0/1 背包问题

**物品每种只有一个，不可分割，要么装进包里，要么不装。**

给定一个背包载重量为`w`和`n`个物品，现在每个物品有两个属性，价值和重量。其中第`i`个物品的重量为`wt[i]`，价值为`val[i]`，现在用这个背包，最多能装的价值是多少。

**状态：**目前数到第`i`个物品，目前背包里的剩余空间

**选择：**1. 装进背包增加钱，减少空间；2. 不装保持不变

定义 `dp[i][j]`： - **`dp[i][j]` 表示前 `i` 个物品、容量 `j` 时的最大价值**。 

### 状态转移方程 

$$
dp[i][j] = \max(dp[i-1][j], dp[i-1][j-w[i]] + v[i])
$$


- **不选物品 `i`**: `dp[i-1][j]`（继承前 `i-1` 个物品的价值）
- **选物品 `i`（前提：`j >= w[i]`）**: `dp[i-1][j - w[i]] + v[i]`（减少 `w[i]` 的容量，增加 `v[i]` 的价值）

### **⚡ 边界条件**
- `dp[0][j] = 0`（没有物品时，价值为 `0`）
- `dp[i][0] = 0`（容量为 `0`，价值为 `0`）

```python
int knapsack(int W, vector<int>& weights, vector<int>& values) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= W; j++) {
            if (j >= weights[i - 1]) {  
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weights[i - 1]] + values[i - 1]);
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }

    return dp[n][W];
}
```

### **时间复杂度**

- **O(N \* W)**，`N` 是物品个数，`W` 是背包容量。

### **空间复杂度**

- **O(N \* W)**，因为使用了 `dp[n][W]` 的二维数组。

 ## 一维 DP（空间优化）

🚀 优化思路：

- `dp[i][j]` 只依赖 `dp[i-1][j]`，可以用 一维数组 `dp[j]` 代替 `dp[i][j]`。
- 逆序遍历 j，避免被覆盖状态。

```c++
int knapsack(vector<int>& weights, vector<int>& values, int W) {
    int n = weights.size();
    vector<int> dp(W + 1, 0);

    for (int i = 0; i < n; i++) {
        for (int j = W; j >= weights[i]; j--) {
            dp[j] = max(dp[j], dp[j - weights[i]] + values[i]);
        }
    }
    return dp[W];
}
```

### **时间复杂度**

- **O(N \* W)**，遍历物品和容量。

### **空间复杂度**

- **O(W)**，用了一维 `dp` 数组。
