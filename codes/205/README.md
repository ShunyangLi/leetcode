# 205. Isomorphic Strings

Given two strings `s` and `t`, *determine if they are isomorphic*.

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

 

**Example 1:**

**Input:** s = "egg", t = "add"

**Output:** true

**Explanation:**

The strings `s` and `t` can be made identical by:

- Mapping `'e'` to `'a'`.
- Mapping `'g'` to `'d'`.

**Example 2:**

**Input:** s = "foo", t = "bar"

**Output:** false

**Explanation:**

The strings `s` and `t` can not be made identical as `'o'` needs to be mapped to both `'a'` and `'r'`.

**Example 3:**

**Input:** s = "paper", t = "title"

**Output:** true

 

**Constraints:**

- `1 <= s.length <= 5 * 104`
- `t.length == s.length`
- `s` and `t` consist of any valid ascii character.

## Solution

没啥太多技巧，直接`hashmap`。

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        if (s.size() != t.size()) return false;

        auto maps = vector<int>(128, -1);
        auto maped = vector<int>(128, 0);

        for (int i = 0; i < s.size(); i ++) {
            int s_ = static_cast<int>(s[i]);
            int t_ = static_cast<int>(t[i]);

            if (maps[s_] == -1) {
                if (maped[t_]) return false;

                maps[s_] = t_;
                maped[t_] = 1;
            } else {
                if (maps[s_] != t_) return false;
            }
        }

        return true;
    }
};
```

