
# Merge Sorted Array

[Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

Given two sorted integer arrays *nums1* and *nums2*, merge *nums2* into *nums1* as one sorted array.

**Note:**

- The number of elements initialized in *nums1* and *nums2* are *m* and *n* respectively.
- You may assume that *nums1* has enough space (size that is greater or equal to *m* + *n*) to hold additional elements from *nums2*.

**Example:**

```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

这是我今天逛[牛客](https://www.nowcoder.com/)的时候发现的一道题，然后想了一下，按照我现在的思想好像只知道直接merge然后sort。还有一种就是正序插入，但是每个element都需要后移一位，但是这样的效率太低了，看了网上的大神的idea之后就发现了新大陆。因为这个list是拍过序的，那么我们就知道了最后一个元素肯定是最大。那么我们为什么不从后面往前面循环呢，这样不需要元素以为，只需要直接赋值就好了。

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    // the last position of array
    int last = nums1.length - 1;
    m--; n--;
    while (m > 0 && n > 0) {
        if (nums1[m] > nums2[n]) nums1[last--] = nums1[m--];
        else nums1[last--] = nums2[n--];
    }

    while (m > 0) nums1[last--] = nums1[m--];
    while (n > 0) nums1[last--] = nums2[n--];
}
```
