
# Palindrome Number

[palindrome number](https://leetcode.com/problems/palindrome-number/)

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example 1:**

```
Input: 121
Output: true
```

**Example 2:**

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

回文数也就是说该数字等于翻转后的数字，`num = reverse(num)`。首先我们可以确定如果该数字是负数直接false就好了。我们可以使用上一题的思路，把数字翻转然后看是否相等。**也可以把边界情况考虑进行。**

```java
public boolean isPalindrome(int x) {
    if (x < 0) return false;

    int reverse = 0, temp = x;

    while (temp != 0) {
        int last_digit = temp % 10;
        reverse = reverse * 10 + last_digit;
        temp = temp / 10;
    }

    return reverse == x;
}
```

# Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

All given inputs are in lowercase letters `a-z`.

集体思路：

采用了垂直对比法。比如：

```
s1 = 'flower'

s2 = 'flow'
s3 = 'flight'

我们第一层循环是表示当前string的index, i = 0;
然后得到第一个string的第i个char
第二个循环式为了和剩余的string比对，
如果i的大小等于当前string的长度也就意味着遇到了结束点，比如当i=3的时候，i = s2.length，也就意味着有结束点了。

c != strs[j].charAt(i) 这就语句是为了判断当前string的第i个char是否和第一个相等，如果不想当也就意味着可以返回i之前的数据


最后一行意味着strs[0]是最短的字符串，而且strs[0]是comm prefix
```

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";

    for (int i = 0; i < strs[0].length(); i ++ ) {
        char c = strs[0].charAt(i);

        for (int j = 1; j < strs.length; j ++) {
            if (i == strs[j].length() || c != strs[j].charAt(i))
                return strs[0].substring(0, i);
        }
    }
    return strs[0];
}
```

也可以试着得到最短的string，然后进行循环，思路差不多：

这种情况其实没什么必要，因为可能没循环结束就return了。

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";

    int min = Integer.MAX_VALUE;
    int index = -1;

    // get the shortest string length
    for (int i = 0; i < strs.length; i ++) {
        if (strs[i].length() < min) {
            min = strs[i].length();
            index = i;
        }
    }

    for (int i  = 0; i < min; i ++) {
        char c = strs[0].charAt(i);

        for (String str : strs) {
            if (c != str.charAt(i)) return str.substring(0, i);
        }
    }

    return strs[index];
}
```