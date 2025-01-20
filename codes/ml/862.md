# 862. Shortest Subarray with Sum at Least K (hard)

Given an integer array `nums` and an integer `k`, return *the length of the shortest non-empty **subarray** of* `nums` *with a sum of at least* `k`. If there is no such **subarray**, return `-1`.

A **subarray** is a **contiguous** part of an array.

 

**Example 1:**

```
Input: nums = [1], k = 1
Output: 1
```

**Example 2:**

```
Input: nums = [1,2], k = 4
Output: -1
```

**Example 3:**

```
Input: nums = [2,-1,2], k = 3
Output: 3
```

## Solution

尝试过sliding window，但是滑动窗口在遇到负数的时候`left`没办法继续收敛了，会出错。官方的教程是使用`prefix sum`和单调队列来实现。`prefix`的作用就是可以快速得到`i`和`j`的sum。而单调队列的作用就是为了筛选掉一些不满足的情况，比如`q[last] >= prefix[i]`，我们需要把队列中的元素`pop`出去，因为如果队列中的元素比正在访问的元素还要大的话，那就证明需要的subarray更长，所以不满足shortest，可以提前排除掉。

### 1. 剔除无效候选
如果当前前缀和 `prefix[j]` 小于或等于队尾索引对应的前缀和 `prefix[dq.back()]`，队尾索引是无效的（无论是否包含负数），因为：
- `prefix[j]` 更小，且索引更靠右，能提供更优解。

这种逻辑确保负数不会干扰队列的有效性。

```cpp
while (!q.empty() && prefix[q.back()] >= prefix[i]) {
    q.pop_back();
}
q.push_back(j);
```

### 2. 快速判断条件

即使负数使得前缀和下降，我们仍可以通过比较 `prefix[j] - prefix[dq.front()]` 是否满足条件 `>= k`，快速找到解。

队列头部始终保存了当前的最小前缀和索引，因此我们可以快速判断是否满足要求，而无需考虑负数的直接影响。

```c++
while (!q.empty() && prefix[j] - prefix[q.front()] >= k) {
    lens = lens > i - q.front() ? i - q.front() : lens;
    q.pop_front();
}
```
最终代码：

```c++
class Solution {
public:
    int shortestSubarray(vector<int>& nums, int k) {
        auto prefix = vector<long>(nums.size() + 1, 0);
        auto q = deque<int>();

        for (int i = 0; i < nums.size(); i ++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }

        int lens = INT_MAX;

        for (int i = 0; i <= nums.size(); i ++) {
            // check the res
            while (!q.empty() && prefix[i] - prefix[q.front()] >= k) {
                lens = lens > i - q.front() ? i - q.front() : lens;
                q.pop_front();
            }

            while (!q.empty() && prefix[q.back()] >= prefix[i]) {
                q.pop_back();
            }

            q.push_back(i);
        }

        return lens == INT_MAX ? -1 : lens;
    }
};
```