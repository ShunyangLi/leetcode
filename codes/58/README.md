
# 58. Length of Last Word

Given a string *s* consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word (last word means the last appearing word if we loop from left to right) in the string.

If the last word does not exist, return 0.

**Note:** A word is defined as a **maximal substring** consisting of non-space characters only.

**Example:**

```
Input: "Hello World"
Output: 5
```

 直接split然后取最后一个，然后直接计算length

```java
public class LengthofLastWord {
    public int lengthOfLastWord(String s) {
        if (s.equals("")) return 0;
        String[] res = s.split(" ");

        if (res.length == 0) return 0;
        return res[res.length - 1].length();
    }

    public static void main(String[] args) {
        String r = "Hello world";

        LengthofLastWord l = new LengthofLastWord();
        System.out.println(l.lengthOfLastWord(r));
    }
}

```