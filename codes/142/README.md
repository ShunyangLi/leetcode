
# 142. Linked List Cycle II

Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

**Note:** Do not modify the linked list.

 

**Example 1:**

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

首先想到的就是HashMap哈哈哈哈，然后他要求说不用额外的空间，在不实用额外的空间的情况下想到了递归+循环的方法，但是时间复杂度比较高。在循环的过程中每个node都进行一个检查环的操作，并且判断在环里面是不是等于该数字，如果不等于的话就代表不是环的一个node，当出现第一个node并且是在环里面的node就代表是环的第一个节点。感觉牺牲了时间换来了空间的优化。。。。。

```java
import java.util.HashMap;

public class LinkedListCycleII {
    public static class ListNode {
        int val;
        ListNode next;
        ListNode() {}
        ListNode(int val) { this.val = val; }
        ListNode(int val, ListNode next) { this.val = val; this.next = next; }
    }

    // hashmap的解决方法
//    public ListNode detectCycle(ListNode head) {
//        HashMap<ListNode, Integer> map = new HashMap<>();
//
//        ListNode curr = head;
//        while (curr != null) {
//            if (map.containsKey(curr)) {
//                return curr;
//            } else {
//                map.put(curr, 1);
//            }
//            curr = curr.next;
//        }
//
//        return null;
//    }

    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) return null;
        ListNode curr = head;

        while (curr.next != null) {
            ListNode temp = loop(curr.next, curr.next.next, curr);
            if (temp != null) return curr;
            curr = curr.next;
        }

        return null;
    }

    public ListNode loop(ListNode slow, ListNode fast, ListNode node) {
        if (slow == null || fast == null || slow.next == null || fast.next == null
                || fast.next.next == null) return null;
        if (slow == fast && slow == node) return slow;
        if (slow == fast) return null;
        return loop(slow.next, fast.next.next, node);
    }


    public static void main(String[] args) {
        ListNode l1 = new ListNode(1);
        ListNode l2 = new ListNode(2);
        ListNode l3 = new ListNode(3);
        ListNode l4 = new ListNode(4);

        l1.next = l2;
        l2.next = l3;
        l3.next = l4;
//        l4.next = l2;

        LinkedListCycleII l = new LinkedListCycleII();
        l.detectCycle(l1);
    }
}
```