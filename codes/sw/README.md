# Sliding window

滑动窗口是一种用于 **数组** 或 **字符串** 处理的优化算法，适用于 **连续子区间** 相关的问题，比如最大/最小子数组和、无重复字符的最长子串等。

### **核心思想**：

1. **定义窗口**：使用两个指针（通常是 `left` 和 `right`）表示窗口的左右边界。
2. **扩展窗口**：移动 `right` 指针扩大窗口，使其满足问题的约束条件。
3. **缩小窗口**：如果窗口不再满足条件，移动 `left` 指针缩小窗口，直到满足条件。
4. **更新结果**：在每次窗口调整时，记录符合要求的解。

### **应用场景**：

- **求最大/最小子数组和**（如：最大连续子数组和 Kadane’s Algorithm）
- **子串匹配**（如：最小覆盖子串）
- **双指针优化**（如：窗口中某个条件满足时的最优解）

滑动窗口适用于 **需要连续处理子数组/子串** 的问题，并且 **可以减少重复计算，提高效率**。

### 基本题型：

1. *Easy*: size fixed, 窗口长度确定，比如max sum of size = k
2. *Median*: size 可变，单限制条件，比如找到subarray sum比目标值大一点或者小一点
3. *Median*: size 可变，双限制条件，比如longest substring with distinct character
4. *Hard*: size fix，单限制条件，比如sliding window maximum

### 解题模板：

```c++
int length_of_longest_substring_distinct(string s, int k) {
    auto maps = unordered_map<char, int>();
    int left = 0, res = 0;

    for (int i = 0; i < s.size(); i ++) {

        // 1. add the element (in)
        if (maps.find(s[i]) == maps.end()) maps[s[i]] = 1;
        else maps[s[i]] += 1;

        // 2. check the size of the maps (out)
        while (maps.size() > k) {
            char const c = s[left];
            maps[c] -= 1;
            if (maps[c] == 0) maps.erase(c);
            left ++;
        }

        // 3. update the result (compute)
        res = max(res, i - left + 1);
    }

    return res;
}
```
