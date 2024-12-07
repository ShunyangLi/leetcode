# 1019. Next Greater Node In Linked List

[Next Greater Node In Linked List](https://leetcode.com/problems/next-greater-node-in-linked-list/) is a medium problem. 
We are given a linked list with `head` as the first node.  Let's number the nodes in the list: `node_1, node_2, node_3, ... etc.`

Each node may have a next larger value: for node_i, next_larger(node_i) is the node_j.val such that `j > i`, `node_j.val > node_i.val,` and `j` is the smallest possible choice.  If such a j does not exist, the next larger value is 0.

Return an array of integers answer, where `answer[i] = next_larger(node_{i+1})`.

Note that in the example inputs (not outputs) below, arrays such as `[2,1,5]` represent the serialization of a linked list with a head node value of 2, second node value of 1, and third node value of 5.
## Approach
This problem is pretty similar with [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) which can be solved by applying stack. For this issue, we can use `stack` + `hashmap` to solve it with $O(n)$ time complexity.
1. Init stack and the final result.
2. `result`(hashmap) will recored the index as key and greater or 0 value as the value
3. When the `temp` node value is greater then the top value of the stack, update the `result` value and pop the top value of stack.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def nextLargerNodes(self, head: ListNode) -> List[int]:
        stack = []
        result = {}
        
        temp = head
        index = 0
        
        # stack store like (val, pos)
        while (temp != None):
            
            while len(stack) != 0 and temp.val > stack[len(stack) - 1][0]:
                result[stack[len(stack) - 1][1]] = temp.val
                stack.pop(len(stack) - 1)
            
            stack.append((temp.val, index))
            result[index] = 0
            
            index += 1
            temp = temp.next
        
        
        return result.values()
```
We can use the following data to give an example:
```txt
Input: [2,1,5]
Output: [5,5,0]
```
In the following part, we will display the `result` and `stack` storage.
```txt
1. stack.push((2, 0)) stack = [(2, 0)], result = {0: 0}

2. stack.push((1, 1)), stack = [(2, 0), (1, 1)], result = {0: 0, 1: 0}

3. The node value is 5, greater than 1, then
    1. top value of stack is (1, 1), then result[1] = node.val (5), stack pop (1, 1)
    2. top value of stack is (2, 0), then result[0] = node.val (5), stack pop (2, 0)
    3. End while loop, and insert the (5, 3) into stack and result.

The final value in result is:
result = {
    0: 5,
    1: 5,
    2: 0
}
```