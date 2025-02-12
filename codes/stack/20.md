
# Valid Parentheses

[valid parentheses](https://leetcode.com/problems/valid-parentheses/)

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```

解题思路：

这个题我使用了stack（栈）的方法来解决这个问题。stack是一种先进后出的数据结构，所以更加适合这种问题。

1. 我们先来初始化一个HashMap来存相对应的括号
2. 先判断该HashMap是否存在该左括号，如果存在就把左括号push到stack里面，
3. 如果不存在先判断stack是否有数据（因为不存在的情况只有该字符是右括号的情况）
4. 然后根据stack pop出来的结果在HashMap找到相对应的右括号，然后判断该字符和HashMap里面的右括号是否匹配。

```java
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<Character>();
    Map<Character, Character> map = new HashMap<>();
    map.put('[', ']');
    map.put('(', ')');
    map.put('{', '}');

    for (int i = 0; i < s.length(); i ++) {
        if (map.containsKey(s.charAt(i))) {
            stack.push(s.charAt(i));
        } else {
            if (stack.isEmpty()) return false;
            if (!map.get(stack.pop()).equals(s.charAt(i))) return false;
        }
    }

    return stack.isEmpty();
}
```
