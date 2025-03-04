# Longest Substring Without Repeating Characters

Given a string, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

解题方法&思路：

首先说下题意，我刚开始是没理解题意来得（暴露了自己的无知╮(╯▽╰)╭），题意就是匹配到最长的子字符串，并且不能重复。比如`pwwkew` 可以匹配到`wke` 最后一个w不可以被匹配到，因为该子字符串已经包含了一个w。

算法解析：~~我们可以使用暴力解法：双循环（但是不推荐)~~。我们可以使用HashMap来解决该问题，因为HashMap查询的时候只需要$O(1)$的时间复杂度。

1. 首先对该字符串进行遍历，如果该字符不在HashMap里面，则把该字符添加到HashMap，取max（HashMap length, ans）
2. 如果该字符已经出现在HashMap里面，从HashMap里面去除一个字符，然后current index保持不变（会对该index进行再次遍历）
3. 循环

有点类似于：当一个字符不在该HashMap，push进去，如果已经在了，pop出来。然后在push之后计算最大的长度是多少。（进去一个如果已经存在就pop出来一个，**因为重点是长度，所以不需要关心到底是哪些字符**）。

Python伪代码

```python
def LongSubString(s):
	while i < len(s):
    if not s[i] in hashmap:
      hashmap[s[i]] = 0
      ans = max(len(hashmap), ans)
    else:
      del hashmap[s[d]]
      d += 1
  return ans
```

Java解决方法：

```java
import java.util.HashMap;
import java.util.Map;

public class LongestSubstring {
    public int lengthOfLongestSubstring(String s) {
        int ans = 0;
        int remove = 0;
        Map<Character, Integer> map = new HashMap<>();

        for (int i = 0; i < s.length();) {
            if (!map.containsKey(s.charAt(i))) {
                map.put(s.charAt(i), 0);
                ans = Math.max(map.size(), ans);
                i ++;
            } else {
                map.remove(s.charAt(remove));
                remove += 1;
            }
        }


        return ans;
    }

    public static void main(String[] args) {
        LongestSubstring lon = new LongestSubstring();
        System.out.println(lon.lengthOfLongestSubstring("abcabcbb"));
    }
}
```