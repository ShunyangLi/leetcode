
# Letter Combinations of a Phone Number

[Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

思路1：

每个数字代表几个相对应的数据，把这些代表的数据进行排列组合。这个第一个想到的就是loop，逐层递进，因为如果是三个数字的话，就是只组合成三个字母的string，可以先把1，2组合得到的结果去和3组合。这样就行了，还有别的算法。

```java
import java.util.*;

public class LetterCombinationsofaPhoneNumber {
    private List<String> letterCombinations(String digits) {
        List<String> result = new LinkedList<>();
        HashMap<Character, String> maps = new HashMap<>();
        maps.put('2', "abc");
        maps.put('3', "def");
        maps.put('4', "ghi");
        maps.put('5', "jkl");
        maps.put('6', "mno");
        maps.put('7', "pqrs");
        maps.put('8', "tuv");
        maps.put('9', "wyxz");

        // 先实验一下相乘的办法，因为还有很多好的办法，一会慢慢看

        for (int i = 0; i < digits.length(); i ++) {
            if (!maps.containsKey(digits.charAt(i))) continue;

            String str = maps.get(digits.charAt(i));
            result = mul(result, new ArrayList<>(Arrays.asList(str.split(""))));
        }

        return result;
    }



    private List<String> mul(List<String> s1, List<String> s2) {
        if (s1.size() == 0 && s2.size() != 0) {
            return s2;
        }

        if (s1.size() != 0 && s2.size() == 0) {
            return s1;
        }

        List<String> result = new LinkedList<>();
        for (String value : s1) {
            for (String s : s2) {
                result.add(value + s);
            }
        }

        return result;
    }

    public static void main(String[] args) {
        LetterCombinationsofaPhoneNumber l = new LetterCombinationsofaPhoneNumber();
        String str = "23";

        List<String> res = l.letterCombinations(str);

        for (String s : res) {
            System.out.println(s);
        }
    }
}

```

思路2：

想学习一下dfs的算法。。。学习使用dfs和递归的方法，这个算法的思路就是直接访问到最底层的str，然后遍历最底层的list，遍历完之后返回到上一层，然后对上一层进行遍历，同时还会使用到最底层的元素。。。递归有点绕。。

![](./11.png)

```java
private List<String> letterCombinations(String digits) {
    List<String> result = new LinkedList<>();

    if (digits == null || digits.length() == 0) return result;

    HashMap<Character, String> maps = new HashMap<>();
    maps.put('2', "abc");
    maps.put('3', "def");
    maps.put('4', "ghi");
    maps.put('5', "jkl");
    maps.put('6', "mno");
    maps.put('7', "pqrs");
    maps.put('8', "tuv");
    maps.put('9', "wyxz");

    dfs(digits, 0, "", result, maps);

    return result;
}

public void dfs(String digits, int index, String temp, List<String> res, HashMap<Character, String> maps) {
    if (index == digits.length()) {
        res.add(temp);
        return;
    }

    String t = maps.get(digits.charAt(index));

    for (int i = 0; i < t.length(); i ++) {
        dfs(digits, index + 1, temp + t.charAt(i), res, maps);
    }

}
```
