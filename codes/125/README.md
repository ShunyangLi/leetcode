
# 125. Valid Palindrome
[Valid Palindrome](https://leetcode.com/problems/valid-palindrome/): Given a string s, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```
This one is pretty easy, we just need to ignore the char which is not belong to `a-z, 0-9`. During the processing, we can use double pointer to do that. Set a index from the left and a index from the right.

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = s.lower()
        start = 0
        end = len(s) - 1

        while start <= end:
            l = s[start]
            r = s[end]

            if not l.isalnum():
                start += 1
                continue

            if not r.isalnum():
                end -= 1
                continue

            if s[start] != s[end]:
                return False

            start += 1
            end -= 1

        return True

```