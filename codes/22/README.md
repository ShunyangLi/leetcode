
# Generate Parentheses

[Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

这个题主要是使用了递归的方法，首先一直添加`(`，直到最深度`n`，如果超过n肯定不合法了，因为左右括号是相对应的。当左括号达到最深度的时候，开始递归右括号，同时右括号的数量应该小于左括号。伪代码:

```python
def find(left, right, depth, str, res):
	if str length == depth * 2:
		res.add(str)
	
	if left < depth:
		find(left+1, right, depth, str+'(' , res)
	
	if right < left:
		find(left, right+1, depth, str+')', res)
```

Java代码：

```java
import java.util.LinkedList;
import java.util.List;

public class GenerateParentheses {
    public List<String> generateParenthesis(int n) {
        List<String> list = new LinkedList<>();
        if (n == 0) return list;

        dfs(0,0,n,list, "");
        return list;
    }

    public void dfs(int close, int open, int depth, List<String> res, String str) {
        if (str.length() == depth * 2) {
            res.add(str);
            return;
        }

        if (open < depth) {
            dfs(close, open + 1, depth, res , str + "(");
        }

        if (close < open) {
            dfs(close + 1, open, depth, res, str + ")");
        }
    }

    public static void main(String[] args) {
        GenerateParentheses ge = new GenerateParentheses();
        List<String> res = ge.generateParenthesis(4);

        for (String str : res) {
            System.out.println(str);
        }
    }
}

```