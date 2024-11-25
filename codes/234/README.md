
# Palindrome Linked List

[Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

Given a singly linked list, determine if it is a palindrome.

**Example 1:**

```
Input: 1->2
Output: false
```

**Example 2:**

```
Input: 1->2->2->1
Output: true
```

**Follow up:**
Could you do it in O(n) time and O(1) space?

思路：

本来想的是用递归的方法算一下和，如果是回文的话结果应该是一样的，想法很美好，现实很残酷。。。LeetCode官方应该也考虑到这个问题了，如果算sum的话会导致整型溢出的问题。。。。不过也算是一个思路吧。o(╥﹏╥)o

```java
public boolean isPalindrome(ListNode head) {
    if (head == null) return false;
    if (head.next == null) return true;
    long [] nums = new long[]{0, 0};
    recur(head,nums);

    return nums[0] == nums[1];
}

public void recur(ListNode curr, long[] nums) {
    if (curr == null) return ;

    nums[0] = nums[0]*10 + curr.val;
    recur(curr.next, nums);
    nums[1] = nums[1]*10 + curr.val;
}

```

最近遇到的题不是递归就是动态规划。。。看来这块有点薄弱，有时间要多练习一下。。看了discussion里面，有个大神写的也是递归的方法，类似于先把第一个指针走到结尾，第二个指针指向head，然后对比是不是一样。感觉很牛逼。。不过递归貌似花费时间有点久。

```java
boolean flag = true;
public boolean isPalindrome(ListNode head) {
    recur(head, head);
    return flag;
}

public ListNode recur(ListNode p1, ListNode p2) {
    if (p1 == null) return p2;
    ListNode node = recur(p1.next, p2);
    if (node.val != p1.val) flag = false;
    return node.next;
}
```

Ps: 还有一种快慢指针的方法。。就是走到中间，然后把两条链表分开对比就行了，但是感觉有点麻烦，还是不写了。。。
