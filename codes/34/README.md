
#  Find First and Last Position of Element in Sorted Array

[ Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

If the target is not found in the array, return `[-1, -1]`.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

使用了一个~二分查找法~，头尾遍历法，不过貌似时间复杂度有点高。。没有别人那么快，花了1ms才通过。一会准备去看看大神的思路，看看有没有好的idea。第23-28行的主要作用是考虑到起始位置和结束位置在同一个地方，当其中有任何一个数字不是-1的时候，代表已经找到这个数字了，但是`head==tail`的时候就会结束循环了，所以会进行一个判断。

```java
public int[] searchRange(int[] nums, int target) {
    if (nums.length == 0) return new int[] {-1, -1};
    if (nums.length == 1)return  (nums[0] == target) ? new int[] {0, 0}: new int[]{-1, -1};

    int head = 0, tail = nums.length - 1;
    int[] range = new int[] {-1, -1};

    while (head <= tail) {
        if (nums[head] == target) {
            range[0] = head;
            if (range[1] != -1) break;
        }

        if (nums[tail] == target) {
            range[1] = tail;
            if (range[0] != -1) break;
        }

        if (nums[head] < target) head ++;
        if (nums[tail] > target) tail --;
    }

    if (head == tail) {
        if (range[0] == -1 || range[1] == -1) {
            range[0] = Math.max(range[0], range[1]);
            range[1] = Math.max(range[0], range[1]);
        }
    }

    return range;
}
```

刚去看了一下discussion里面，然后发现自己写的二分查找不叫二分查找，只能算是一个两头遍历的循环。然后根据二分查找的方法写了一份：

```java
public int[] searchRange(int[] nums, int target) {
    if (nums.length == 0) return new int[] {-1, -1};

    int head = 0, tail = nums.length - 1;
    int[] range = new int[] {-1, -1};

    while (head <= tail) {
        int mid = (head + tail) / 2;

        if (nums[mid] > target) {
            tail = mid - 1;
        } else if (nums[mid] < target) {
            head = mid + 1;
        } else {
            // make a loop from head to tail and tail to end
            while (head <= mid) {
                if (nums[head] == target) {
                    range[0] = head;
                    break;
                }
                head ++;
            }

            while (tail >= mid) {
                if (nums[tail] == target) {
                    range[1] = tail;
                    break;
                }
                tail --;
            }
            break;
        }
    }

    return range;
}
```