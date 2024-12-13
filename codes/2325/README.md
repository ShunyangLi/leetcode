# 2325. Decode the Message

You are given the strings `key` and `message`, which represent a cipher key and a secret message, respectively. The steps to decode `message` are as follows:

1. Use the **first** appearance of all 26 lowercase English letters in `key` as the **order** of the substitution table.
2. Align the substitution table with the regular English alphabet.
3. Each letter in `message` is then **substituted** using the table.
4. Spaces `' '` are transformed to themselves.

- For example, given `key = "**hap**p**y** **bo**y"` (actual key would have **at least one** instance of each letter in the alphabet), we have the partial substitution table of (`'h' -> 'a'`, `'a' -> 'b'`, `'p' -> 'c'`, `'y' -> 'd'`, `'b' -> 'e'`, `'o' -> 'f'`).

Return *the decoded message*.

## Solution

没啥太大难度，hashmap。

```c++
class Solution {
public:
    string decodeMessage(string key, string message) {
        auto maps = unordered_map<char, char>();

        char x = 'a';

        for (char const& k : key) {
            if (k == ' ') continue;
            if (maps.find(k) != maps.end()) continue;

            maps[k] = x;
            
            x += 1;
        }

        string res = "";

        for (char const & m : message) {
            res += m == ' ' ? m : maps[m];
        }

        return res;
    }
};
```

