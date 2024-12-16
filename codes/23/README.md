# 23. Merge k Sorted Lists (Hard)

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

 

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

 

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `-104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `104`.

## Solution

21年的时候尝试过做这个题，好像没做出来。但现在看一下好像就是priority queue就能解决，好像不难。

```c++
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto pq = priority_queue<ListNode*, vector<ListNode*>, decltype([] (ListNode* a, ListNode *b) -> bool {return a->val > b->val;})>();

        ListNode* head = nullptr;
        ListNode* curr = nullptr;
        
        for (auto& node : lists) {

            while (node != NULL) {
                pq.push(node);
                node = node -> next;
            }
        }
        
        while (!pq.empty()) {
            auto top = pq.top();
            pq.pop();
            
            if (head == nullptr) {
                head = top;
                curr = top;
            } else {
                curr->next = top;
                curr = top;
            }
        }

        if (curr != NULL) curr->next = NULL;
        
        
        return head;
    }
};
```

