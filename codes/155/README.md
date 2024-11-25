
# Min Stack

[Min Stack]()(https://leetcode.com/problems/min-stack/)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

 

**Example 1:**

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

这个只需要知道stack是先进后出的原则就行。

```java
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;

public class MinStack {
    /** initialize your data structure here. */
    private List<Integer> list;
    private int lens;

    public MinStack() {
        this.list = new LinkedList<>();
        this.lens = 0;
    }

    public void push(int x) {
        this.list.add(x);
        this.lens ++;
    }

    public void pop() {
        if (this.list.size() > 0) {
            this.lens --;
            this.list.remove(this.lens);
        }
    }

    public int top() {
        if (this.list.size() > 0) {
            return this.list.get(this.lens - 1);
        }
        return -1;
    }

    public int getMin() {
        return Collections.min(this.list);
    }

    public static void main(String[] args) {
        MinStack m = new MinStack();
        m.push(-2);
        m.push(0);
        m.push(-3);
        System.out.println(m.getMin());
        m.pop();
        System.out.println(m.top());   // return 0
        System.out.println(m.getMin()); // return -2
    }
}

```
