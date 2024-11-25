
# 387. First Unique Character in a String

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

**Examples:**

```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

 使用了HashMap的方法，当字符只出现一次就设置key为当前char，value设置为index，如果出现超过一次，就标记value是MAX。筛选HashMap里面value不是MAX，并且返回就行了。

```java
import java.util.Collections;
import java.util.HashMap;

public class FirstUnique {
    public int firstUniqChar(String s) {
        HashMap<Character, Integer> map = new HashMap<>();

        for (int i = 0; i < s.length(); i ++) {
            char c = s.charAt(i);
            if (map.containsKey(c)) {
                map.put(c, Integer.MAX_VALUE);
            } else {
                map.put(c, i);
            }
        }

        int position = Integer.MAX_VALUE;
        for (Character c : map.keySet()) {
            if (map.get(c) != Integer.MAX_VALUE) {
                if (map.get(c) < position) position = map.get(c);
            }
        }
        if (position == Integer.MAX_VALUE) return -1;
        return position;
    }

    public static void main(String[] args) {
        FirstUnique f = new FirstUnique();
        String str = "loveleetcode";
        System.out.println(f.firstUniqChar(str));
    }
}
```