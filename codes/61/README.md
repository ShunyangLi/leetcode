
# 61. Rotate List

Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.

**Example 1:**

```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

**Example 2:**

```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

做法稍微蠢了一点，我是通过先计算链表的长度，然后把`k = k % length`这样就可以得到相对链表来说的第几位。然后遍历链表，当节点的index等于k的时候开始把k之后的node放到链表的起始位置。这样就完成了rotate list。

![](/17.png)

```java
public class RotateList {
    public static class ListNode {
        int val;
        ListNode next;
        ListNode() {}
        ListNode(int val) { this.val = val; }
        ListNode(int val, ListNode next) { this.val = val; this.next = next; }
    }

    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;
        ListNode curr = head;

        int length = length(curr);
        k = length - (k % length);
        if(k == length) return head;

        ListNode rota = find_rota(curr, k);
        ListNode temp = rota;

        while (temp.next != null) {
            temp = temp.next;
        }
        temp.next = head;
        head = rota;


        return head;
    }

    public ListNode find_rota(ListNode curr, int k) {
        while (k > 1) {
            curr = curr.next;
            k --;
        }

        ListNode node = curr.next;
        curr.next = null;
        return node;

    }

    public int length(ListNode node) {
        int length = 0;
        while (node != null) {
            node = node.next;
            length ++;
        }

        return length;
    }
    public static void main(String[] args) {
        ListNode l1 = new ListNode(1);
        ListNode l2 = new ListNode(2);
        ListNode l3 = new ListNode(3);
//        ListNode l4 = new ListNode(4);
//        ListNode l5 = new ListNode(5);

        l1.next = l2;
        l2.next = l3;
//        l3.next = l4;
//        l4.next = l5;

        RotateList r = new RotateList();
        r.rotateRight(l1, 2);
    }
}

```