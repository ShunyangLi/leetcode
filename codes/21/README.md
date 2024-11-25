
# Merge Two Sorted Lists

[Merge two sorted lists](https://leetcode.com/problems/merge-two-sorted-lists/)

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

这个好像没什么说的，就直接对比大小然后merge在一起就行

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode head = new ListNode();
    ListNode temp = head;

    while (l1 != null && l2 != null) {
        temp.next = new ListNode();
        temp = temp.next;

        if (l1.val <= l2.val) {
            temp.val = l1.val;
            l1 = l1.next;
        } else {
            temp.val = l2.val;
            l2 = l2.next;
        }
    }

    if (l1 != null) temp.next = l1;
    if (l2 != null) temp.next = l2;

    return head.next;
}
```