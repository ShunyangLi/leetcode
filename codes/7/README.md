
# Reverse Integer

[reverse integer](https://leetcode.com/problems/reverse-integer/)

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

本来以为这个题很简单，使用了最简单的算法，但是呢，出现了一个整型溢出的问题o(╥﹏╥)o。

先解析一下思路吧：

1. 首先我们可以用`x%10`来得到最后一个数字（因为数字不可能大于10）
2. `x/10`是把最后一位数去掉
3. `reverse*10`是为了整体向左移一位，比如(44, 44*10 = 440)多出一个空位，然后加上`x%10`的数字

```java
public int reverse(int x) {
    int reverse = 0;

    while (x != 0) {
        int last_digit = x % 10;
        reverse = reverse * 10 + last_digit;
        x = x / 10;
    }

    return reverse ;
}
```

这样如果数字区间在$[−2^{31}, 2^{31} − 1]$的话那么就会出现溢出的问题了，大家可以使用`1534236469`来尝试一下。

所以这时候我们就需要来做些判断了：

当`reverse > intMAX/10`的时候我们基本上就可以确定他会溢出了，比如说`intMAX = 2147483647`，假设我们的`reverse = 214748365` 这时候`intMAX/10 =214748364 `，`reverse > intMAX`。这时候如果`reverse * 10`我们不管最后一个数字是什么了，`reverse`已经超出了int取值范围。

正确代码：

```java
public int reverse(int x) {
    int reverse = 0;

    while (x != 0) {
        int last_digit = x % 10;
        if (reverse > Integer.MAX_VALUE / 10) return 0;
        if (reverse < Integer.MIN_VALUE / 10) return 0;
        reverse = reverse * 10 + last_digit;
        x = x / 10;
    }

    return reverse ;
}
```