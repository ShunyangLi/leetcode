# 86. Partition List

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

```
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
```

**Example 2:**

```
Input: head = [2,1], x = 2
Output: [1,2]
```

 ## Solution

不难，记录一下访问到的nodes就行， 或者可以通过动态的记录两条linked list然后合并，但是这种容易出错，有时间可以尝试实现一下。

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
    ListNode* partition(ListNode* head, int x) {
        auto sm = vector<ListNode*>();
        auto lg = vector<ListNode*>();

        auto curr = head;

        while (curr != NULL) {
            if (curr->val < x) {
                sm.push_back(curr);
            } else {
                lg.push_back(curr);
            }

            curr = curr->next;
        }

        if (!sm.empty()) head = sm[0];
        if (sm.empty() && !lg.empty()) head = lg[0];

        curr = NULL;
        for (int i = 0; i < sm.size(); i ++) {
            if (curr == NULL) curr = sm[i];
            else {
                curr->next = sm[i];
                curr = sm[i];
            }
        }

        for (int i = 0; i < lg.size(); i ++) {
            if (curr == NULL) curr = lg[i];
            else {
                curr->next = lg[i];
                curr = lg[i];
            }
        }

        if (curr != NULL) curr->next = NULL;

        return head;
    }
};
```

