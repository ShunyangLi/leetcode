
# Group Anagrams

[Group Anagrams](https://leetcode.com/problems/group-anagrams/)

Given an array of strings, group anagrams together.

**Example:**

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:**

- All inputs will be in lowercase.
- The order of your output does not matter.

把使用同一个字符构成的string放到一起。首先确定是使用HashMap，在遍历数组的时候对每一个string先进行sort一下（或者用hash的方法，我下面有写），排过序的string当做key，如果还有相同的数据就接着添加，如果没有该key就初始化一个数组。

```java
import java.util.*;

public class GroupAnagrams {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> list = new LinkedList<>();
        HashMap<String, List<String>> hashMap = new HashMap<>();
        for (String str : strs) {
            hash(str);
            char[] temp_char = str.toCharArray();
            Arrays.sort(temp_char);
            String key = Arrays.toString(temp_char).toString();
            if (hashMap.containsKey(key)) {
                hashMap.get(key).add(str);
            } else {
                List<String> temp = new LinkedList<>();
                temp.add(str);
                hashMap.put(key, temp);
            }
        }

        for (String key : hashMap.keySet()) {
            list.add(hashMap.get(key));
        }
        return list;
    }

    public void hash(String str) {
        int hash = 31;
        for (int i = 0; i < str.length(); i ++) {
            hash += str.charAt(i);
        }
        System.out.println(hash);
    }


    public static void main(String[] args) {
        String[] strs = new String[] {"duy","ill"};
        GroupAnagrams ga = new GroupAnagrams();
        ga.groupAnagrams(strs);
    }
}

```