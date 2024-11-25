
# Move Zeroes

[Move Zeroes](https://leetcode.com/problems/move-zeroes/)

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note**:

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

一开始看到这个想了一下双指针，一个从头开始一个从尾部开始，这样虽然可以把0都放到后面，但是这样就不是顺序来的了。。。看了一下大佬思路，也是双指针（方法思路没错对吧），只不过这个双指针都是从头开始的。先设置一个0的指针`zero=-1`因为我们一开始不知道0的位置在那，然后开始循环，如果该数字为0可以对`zero`进行赋值了（`zero == -1`），当`zero != -1`说明之前已经有了0的数字。然后如果该数字不等于0的时候表示可以对之前的zero的index进行替换，前提是`zero != -1`，然后判断zero和i的位置，如果i是zero的next说明是只有一个zero，不然的话就代表zero后面还是一个0的数字，所以这时候`zero++`就行。

```java
public void moveZeroes(int[] nums) {
    int zero = -1;

    for (int i = 0; i < nums.length; i ++) {
        if (nums[i] != 0) {
            if (zero == -1) continue;
            nums[zero] = nums[i];
            nums[i] = 0;
            zero = (zero == i + 1) ? i : zero+1;
        } else {
            if (zero != -1) continue;
            zero = i;
        }
    }
}
```