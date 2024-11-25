
# 48. Rotate Image

You are given an *n* x *n* 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

**Note:**

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

emmm这个题我使用了另一个double array应该是不咋对

```java
import java.util.Arrays;

public class RotateImage {
    public void rotate(int[][] matrix) {
        int[][] res = new int[matrix.length][matrix[0].length];

        for (int i = 0; i < matrix[0].length; i ++) {
            int index = 0;
            for (int j = matrix.length - 1; j >= 0; j --) {
                res[i][index++] = matrix[j][i];
            }
        }

        for (int i = 0; i < res.length; i ++) {
            matrix[i] = res[i];
        }
    }

    public static void main(String[] args) {
        int[][] m = new int[][] {{1,2,3},{4,5,6},{7,8,9}};
        RotateImage ri = new RotateImage();
        ri.rotate(m);

        for (int[] ints : m) {
            System.out.println(Arrays.toString(ints));
        }
    }
}
```