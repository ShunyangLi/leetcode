# 232. Implement Queue using Stacks

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

**Example 1:**

```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

 

**Constraints:**

- `1 <= x <= 9`
- At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
- All the calls to `pop` and `peek` are valid.

## Solution

`Queue`的原理是先进先出，`Stack`的原理是先进后出。所以`queue.top()`就是`stack`最底层的元素。那么我们就可以使用两个`stack`来完成该任务。

```c++
class MyQueue {
private:
    stack<int> s1, s2;
public:
    MyQueue() {
        s1 = stack<int>();
        s2 = stack<int>();
    }
    
    void push(int x) {
        s1.push(x);
    }
    
    int pop() {
        while (!s1.empty()) {
            s2.push(s1.top());
            s1.pop();
        }

        int val = s2.top();
        s2.pop();

        while (!s2.empty()) {
            s1.push(s2.top());
            s2.pop();
        }
        return val;
    }
    
    int peek() {
        while (!s1.empty()) {
            s2.push(s1.top());
            s1.pop();
        }

        int val = s2.top();

        while (!s2.empty()) {
            s1.push(s2.top());
            s2.pop();
        }

        return val;
    }
    
    bool empty() {
        return s1.empty() && s2.empty();
    }
};
```

