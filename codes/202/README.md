
# Happy Number

[Happy Number](https://leetcode.com/problems/happy-number/)

Write an algorithm to determine if a number `n` is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1. Those numbers for which this process **ends in 1** are happy numbers.

Return True if `n` is a happy number, and False if not.

**Example:** 

```
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

输入一个数字，然后把数字分开，然后平方相加。比如`19 = 1^2 + 9^2`得出的数字再继续进行一样的操作，一直简化到该数字到个位数，如果该数字等于1或者7的时候，就可以return true，否则的话就return false

```java
import java.util.LinkedList;
import java.util.List;

public class HappyNumber {
    public boolean isHappy(int n) {
        if (n == 1) return true;
        int sum = n;
        List<Integer> list = new LinkedList<>();

        while (true) {
            list = split(sum);
            sum = 0;
            for (Integer i : list) {
                sum += Math.pow(i, 2);
            }

            if (sum == 1 || sum == 7) {
                return true;
            }

            if (sum < 10) return false;

        }
    }

    public List<Integer> split(int n) {
        List<Integer> list = new LinkedList<>();
        while (n != 0) {
            list.add(n % 10);
            n = n / 10;
        }
        return list;
    }

    public static void main(String[] args) {
        HappyNumber hp = new HappyNumber();
        System.out.println(hp.isHappy(19));
    }
}

```