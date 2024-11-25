
# Implement strStr()

[Implement strStr()](https://leetcode.com/problems/implement-strstr/)

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**返回要求的是index，我还以为是长度一度迷茫为啥不对o(╥﹏╥)o**，一个for就能解决的问题，我们先去`needle`的第一个字符，然后在循环`haystack`的时候如果里面有字符和`needle`的第一个字符匹配了就用`substring`来接取相对应的长度，然后判断是否和`needle`相等（需要判断index的长度是不是超过了`haystack`的长度）。

```java
public int strStr(String haystack, String needle) {
    if (needle.length() == 0) return 0;
    char c = needle.charAt(0);
    for (int i = 0; i < haystack.length(); i ++) {
        if (haystack.charAt(i) == c) {
            if (i + needle.length() > haystack.length()) return -1;
            if (haystack.substring(i, i+needle.length()).equals(needle)) return i;
        }
    }
    return -1;
}
```
