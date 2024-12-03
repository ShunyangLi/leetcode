Given a `sentence` that consists of some words separated by a **single space**, and a `searchWord`, check if `searchWord` is a prefix of any word in `sentence`.

Return *the index of the word in* `sentence` *(**1-indexed**) where* `searchWord` *is a prefix of this word*. If `searchWord` is a prefix of more than one word, return the index of the first word **(minimum index)**. If there is no such word return `-1`.

A **prefix** of a string `s` is any leading contiguous substring of `s`.

 

**Example 1:**

```
Input: sentence = "i love eating burger", searchWord = "burg"
Output: 4
Explanation: "burg" is prefix of "burger" which is the 4th word in the sentence.
```

**Example 2:**

```
Input: sentence = "this problem is an easy problem", searchWord = "pro"
Output: 2
Explanation: "pro" is prefix of "problem" which is the 2nd and the 6th word in the sentence, but we return 2 as it's the minimal index.
```

**Example 3:**

```
Input: sentence = "i am tired", searchWord = "you"
Output: -1
Explanation: "you" is not a prefix of any word in the sentence.
```

 

**Constraints:**

- `1 <= sentence.length <= 100`
- `1 <= searchWord.length <= 10`
- `sentence` consists of lowercase English letters and spaces.
- `searchWord` consists of lowercase English letters.



## Solution

不是特别难，注意下边界情况就行。python可以直接split，但没啥意思。

```c++
class Solution {
public:
    int isPrefixOfWord(string sentence, string searchWord) {
        int wordIndex = 0, wordCount = 0;
        bool isNewWord = true;

        for (int i = 0; i < sentence.size(); ++i) {
            if (sentence[i] == ' ') {
                isNewWord = true; // Start of a new word
                continue;
            }

            if (isNewWord) {
                wordCount++; // Increment word count
                isNewWord = false;

                int j = 0;
                while (j < searchWord.size() && i + j < sentence.size() && sentence[i + j] == searchWord[j]) {
                    j++;
                }

                if (j == searchWord.size()) return wordCount; // Found prefix match
                if (i + j < sentence.size() && sentence[i + j] != ' ') i += j - 1; // Skip remaining characters in this word
            }
        }

        return -1; // No match found
    }
};
```

