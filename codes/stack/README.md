# 栈（Stack)

栈（Stack）是一种**后进先出（LIFO, Last In First Out）**的数据结构，具有**`push`（入栈）**和**`pop`（出栈）**两种基本操作。

------

### **栈的基本操作**

| 操作        | 说明           | Python 代码       |
| ----------- | -------------- | ----------------- |
| `push(x)`   | 元素 `x` 入栈  | `stack.append(x)` |
| `pop()`     | 移除栈顶元素   | `stack.pop()`     |
| `top()`     | 获取栈顶元素   | `stack[-1]`       |
| `isEmpty()` | 判断栈是否为空 | `not stack`       |

------

### **栈的应用场景**

1. **括号匹配**（如 `()`、`[]`）
2. **单调栈**（求下一个更大/更小元素）
3. **DFS 遍历**（避免递归栈溢出）
4. **计算器问题**（后缀表达式求值）

### **简单示例**

**示例：括号匹配**

```python
pythonCopydef isValid(s: str) -> bool:
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    for ch in s:
        if ch in mapping:
            if not stack or stack.pop() != mapping[ch]:
                return False
        else:
            stack.append(ch)
    return not stack
```

# Calculator

这个题的话就是把给出的string运算出结果，比如`'1+2+3'`需要把这个string都转成数字然后进行运算，最基础的问题可以不使用数据结构来运算，直接做一些简单的操作就行。进阶版本是添加上`( )`，这一类的操作，这样的话我们可以使用`stack`进行运算，当遇到`(`的时候我们可以当作重新开启了一个`stack`直到括号内的元素都运算完成再把`stack`内的元素进行合并计算。类似的如果运算中出现优先级符号`*/`，我们同样可以使用`stack`。

- [224. Basic Calculator](https://leetcode.com/problems/basic-calculator/)
- [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)
