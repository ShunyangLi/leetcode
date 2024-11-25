
# Add Two Numbers

[Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

算法思想：

**主要应该考虑一下边界情况：**

1. 当sum>=10 
2. 两个链表不一样长
3. 在最后一个数字相加的时候sum>=10

第6行的while语句就是为了两条链表不一样长度。第7-8行判断如果该链表不为null就取该val，不然的话就为0（为了不影响下面的计算）。`int overflow = 0`是为了解决和超过10的情况，第9行把溢出的数字和v1，v2相加，然后把该结果取余就是结果。然后把`overflow = sum/10` 得到进位数。每次生成新的数字之后生成一个新的node没然后链接起来。**主要是考虑不同的边界情况。**

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    int overflow = 0;
    if (l1 == null && l2 == null) return null;
    ListNode new_node = new ListNode();
    ListNode head = new_node;
    while (l1 != null || l2 != null) {
        int v1 = (l1 != null) ? l1.val : 0;
        int v2 = (l2 != null) ? l2.val : 0;
        int sum = v1+v2 + overflow;
        overflow = sum / 10;
        if (l1 != null) l1 = l1.next;
        if (l2 != null) l2 = l2.next;
        new_node.val = sum % 10;
        if (l1 != null || l2 != null) {
            new_node.next = new ListNode();
            new_node = new_node.next;
        }
    }
    if (overflow > 0) {
        new_node.next = new ListNode(overflow % 10);
    }
    return head;
}
```