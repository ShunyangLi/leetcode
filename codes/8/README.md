
# String to Integer (atoi)

[String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

- Only the space character `' '` is considered as whitespace character.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.

**Example 1:**

```
Input: "42"
Output: 42
```

**Example 2:**

```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

**Example 3:**

```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

**Example 4:**

```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

**Example 5:**

```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.
```

这是我写LeetCode以来submit最多的次数╮(╯▽╰)╭，里面条件的限制和符号的判定太烦了。给大家写一下判断条件：

1. 空格只能出现在最前面，一旦出现了-+0-9这些符号，之后再遇到空格就要break
2. -+符号考虑到符号的问题
3. 只能以数字开头的才能提取数字，如果不是就return 0
4. 考虑到整型溢出的问题

就是判断条件有点麻烦，其他的还好，我写的有点乱七八糟的，讲究看下。。。如果有好的idea可以写在comment里面，我会修改的。

```java
public int myAtoi(String str) {
    int res = 0;
    // + or -
    int operation = 1;
    if (str.equals(""))return 0;
    boolean whitespace = true;
    boolean sign = true;

    for (int i = 0; i < str.length(); i ++) {
        char c = str.charAt(i);
        if (whitespace && c == ' ') continue;
        if (!whitespace && c == ' ') break;
        if ((c == '-' || c == '+') && sign) {
            operation = (c == '-')?-1:1;
            sign = false;
            whitespace=false;
            continue;
        }
        if (c < '0' || c > '9') break;
        whitespace = false;
        sign=false;
        if (res*operation > Integer.MAX_VALUE/10 || res*operation < Integer.MIN_VALUE/10) {
            if (operation > 0) return Integer.MAX_VALUE;
            else return Integer.MIN_VALUE;
        }

        if (res*operation == Integer.MAX_VALUE/10) if ((int)c-48 >= Integer.MAX_VALUE %10) return Integer.MAX_VALUE;
        if (res*operation == Integer.MIN_VALUE/10) if ((int)(c-48) * -1 <= Integer.MIN_VALUE % 10) return Integer.MIN_VALUE;
        res = res * 10 + (int) c - 48;
    }
    res *= operation;
    return res;
}
```