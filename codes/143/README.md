# 143. Reorder List

You are given the head of a singly linked-list. The list can be represented as:

```
L0 → L1 → … → Ln - 1 → Ln
```

*Reorder the list to be on the following form:*

```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[1, 5 * 104]`.
- `1 <= Node.val <= 1000`

## Solution

使用`stack`，先把所有node都收集起来，然后开始pop。考虑一下边界情况就行，不难。

```c++
class Solution {
public:
    void reorderList(ListNode* head) {

        if (head->next == nullptr) return;

        auto stack = vector<ListNode*>();

        ListNode* tmp = head;

        while (tmp != nullptr) {
            stack.push_back(tmp);
            tmp = tmp->next;
        }

        int lens = stack.size();
        tmp = head;

        while (!stack.empty()) {
            auto ele = stack.back();
            stack.pop_back();

            auto n = tmp -> next;

            tmp -> next = ele;
            ele -> next = n;
            tmp = n;

            if (stack.size() == int(lens / 2)) break;
        }

        tmp -> next -> next = nullptr;

    }
};
```

