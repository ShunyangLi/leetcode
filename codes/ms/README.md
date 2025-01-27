# 单调栈（Monotonic stack）

就是栈中元素，按照递增顺序或者递减顺序排列的时候（是否严格递增或者递减取决于具体题目）。

优势：时间复杂度是线性的，每个元素遍历一次。

**基础模板：**

```c++
auto nextGreater(vector<int>& nums) -> vector<int> {
	auto res = vector<int>(nums.size());

    auto stack = stack<int>();

    for (int i = nums.size() - 1; i >= 0; i --) {
        while (!stack.empty() && nums[i] >= stack.back()) stack.push_back();
        res[i] = stack.empty() ? - 1 : stack.back();
        stack.push(nums[i]);
    }

    return res;
}

```
