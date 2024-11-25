
# Single Number

Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1
```

**Example 2:**

```
Input: [4,1,2,1,2]
Output: 4
```

庆幸一下终于可以独立思考出来一些简单的算法了。这个题其实算是还OK的，可以使用HashMap解决，因为HashMap监测key的时候的时间复杂度是$O(1)$，所以整体时间是$O(n)$。在loop里面去判断该数字是否存在HashMap里面，如果不存在就push进去，如果已经存在就删除。那么最后剩下的那个元素肯定是single（~~单身狗~~）。

```java
public int singleNumber (int[] A) {
    // write code here
    Map<Integer, Integer> map = new HashMap<>();
    for (int value : A) {
        if (map.containsKey(value)) {
            map.remove(value);
        } else {
            map.put(value, value);
        }
    }
    return map.get(map.keySet().toArray()[0]);
}
```

Ps: 在discuss里面看到了一个特别骚的操作。。。真的骚操作。。。

使用了异或（exclusive OR简称xor）这个来判断的，异或的话空间复杂度只有$O(1)$。

>$1\oplus0=1$
>
>$1\oplus1=1$
>
>$0\oplus0=0$
>
>$0\oplus1=1$

```java
public int singleNumber(int[] nums) {
    int res = 0;
    for (int i = 0; i < nums.length; i++) {
        res ^= nums[i];
    }
    return res;
}
```

行吧。。。牛逼。。