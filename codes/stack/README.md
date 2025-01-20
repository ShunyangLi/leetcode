# Calculator

这个题的话就是把给出的string运算出结果，比如`'1+2+3'`需要把这个string都转成数字然后进行运算，最基础的问题可以不使用数据结构来运算，直接做一些简单的操作就行。进阶版本是添加上`( )`，这一类的操作，这样的话我们可以使用`stack`进行运算，当遇到`(`的时候我们可以当作重新开启了一个`stack`直到括号内的元素都运算完成再把`stack`内的元素进行合并计算。类似的如果运算中出现优先级符号`*/`，我们同样可以使用`stack`。

- [224. Basic Calculator](https://leetcode.com/problems/basic-calculator/)
- [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)