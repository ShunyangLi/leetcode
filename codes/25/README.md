# 25. Reverse Nodes in k-Group (hard)

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return *the modified list*.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

 

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

 

**Follow-up:** Can you solve the problem in `O(1)` extra memory space?



## Solution

有好几种不同的做法，可以使用`vector`来记录位置，然后在vector里面进行交换。但是这种方法空间复杂度`O(n)`，另一种方法就是使用递归，当遍历到第`k`个的时候开始调用回溯算法进行reverse。

### S1

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {

        if (head == NULL || k <= 1) return head;

        auto vec = vector<ListNode*>();

        auto rec = vector<int>();
        rec.push_back(0);

        ListNode* curr = head;

        while (curr != NULL) {
            vec.push_back(curr);
            if (vec.size() % k == 0) rec.push_back(vec.size());
            curr = curr->next;
        }

        // reverse the linked list by ranges
        for (int i = 1; i < rec.size(); i ++) {
            int s = rec[i - 1], e = rec[i] - 1;

            while (s < e) {
                auto tmp = vec[s];
                vec[s] = vec[e];
                vec[e] = tmp;

                s ++;
                e --;
            }
        }

        for (int i = 0; i < vec.size(); i++) {
            if (i == vec.size() - 1) vec[i]->next = NULL;
            else vec[i]->next = vec[i + 1];
        }

        return vec[0];
        
    }
};
```

### S2

```c++
class Solution {
public:
    ListNode* reverse(ListNode* head) {
        if (head == NULL || head->next == NULL) return head;

        auto last = reverse(head->next);

        head->next->next = head;
        head->next = NULL;

        return last;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        
        int cnt = 0;

        ListNode* curr = head;
        ListNode* prev = NULL;
        ListNode* thead = NULL;

        // curr = head;
        head = NULL;
        
        while (curr != NULL) {
            if (thead == NULL) thead = curr;
            cnt ++;

            if (cnt % k == 0) {
                auto tmp = curr->next;
                curr->next = NULL;

                auto n = reverse(thead);
                if (head == NULL) head = n;

                if (prev == NULL) prev = thead;
                else {
                    prev -> next = n;
                    prev = thead;
                }

                thead->next = tmp;

                thead = NULL;

                curr = tmp;

                continue;
            }


            curr = curr->next;
        }

        return head;
        
    }
};
```

