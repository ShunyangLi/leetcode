
# Median of two sorted array

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume **nums1** and **nums2** cannot be both empty.

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

没办法太菜了找不到合适的方法来写了，有想过分开循环，但是这样需要有很多的判断条件，晚会会再尝试的，目前就快的方法就是合并array了。

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {

    List<Integer> list = new LinkedList<>();
    int l1 = 0, l2 = 0;

    while (l1 < nums1.length && l2 < nums2.length) {
        if (nums1[l1] < nums2[l2]) {
            list.add(nums1[l1]);
            l1 += 1;
        } else {
            list.add(nums2[l2]);
            l2 += 1;
        }
    }

    // add n2
    if (l1 >= nums1.length) {
        for (int i = l2; i < nums2.length; i ++) list.add(nums2[i]);
    } else {
        for (int i = l1; i < nums1.length; i ++) list.add(nums1[i]);
    }

    int len = list.size();
    if (len % 2 != 0) return list.get(len/2);
    return (double) (list.get(len/2) + list.get(len/2-1)) / 2;
}

```