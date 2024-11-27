# 126. Word Ladder II

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *all the **shortest transformation sequences** from* `beginWord` *to* `endWord`*, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words* `[beginWord, s1, s2, ..., sk]`.

 

**Example 1:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
Explanation: There are 2 shortest transformation sequences:
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"
```

**Example 2:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: []
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```

 

**Constraints:**

- `1 <= beginWord.length <= 5`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 500`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are **unique**.
- The **sum** of all shortest transformation sequences does not exceed `105`.

## Solution

这个题代码量很大，很复杂，核心原理是使用`bi-BFS`解决的。

```c++
class Solution {
public:
    vector<vector<string>> findLadders(string from, string to, vector<string>& wordList) {
        
        unordered_set<string> lexicon(wordList.begin(), wordList.end());
		auto result = std::vector<std::vector<std::string>>{};

		// check whether exist to, if not just return a empty
		if (not lexicon.count(to)) {
            return result;
        }

		// init a word maps for double bfs
		auto word_maps = unordered_map<std::string, std::vector<std::string>>{};
		// the left vector
		auto left = unordered_set<std::string>{from};
		// the right vector
		auto right = unordered_set<std::string>{to};
		// contain all the visited elements
		auto visited = unordered_set<std::string>{};

		// the bfs direction, could start at from and start at end
		// true means start at from
		// false means start at end
		// this is helpful for store the word into hashmap
		auto ahead = true;
		auto found = false;

		// which contain all the words
		while (not left.empty() and not right.empty()) {
			// change the position when left > right
			// part of two end bfs algorithm
			if (left.size() > right.size()) {
				std::swap(left, right);
				ahead = not ahead;
			}

			// contain all the words which generate according to left set words
			// which like insert to queue in bfs
			auto queue = unordered_set<std::string>{};

			// insert all the left set into visited
			visited.insert(left.begin(), left.end());

			// loop all the element in the left
			for (auto origin_word : left) {
				auto neighbors = neighbor(origin_word, visited, lexicon);
				for (auto const& word : neighbors) {
					if (std::find(right.begin(), right.end(), word) != right.end()) {
						found = true;
					}
					if (ahead) {
						word_maps[word].push_back(origin_word);
					}
					else {
						word_maps[origin_word].push_back(word);
					}
					queue.insert(word);
				}
			}
			// put the new path to left then do new iterator
			left = queue;

			// if found the to word, then generate the word ladders
			if (found) {
				result.push_back(std::vector<std::string>{to});
				// the first one should be from
				while (result[0][0] != from) {
					result = generate_result(result, word_maps);
				}
				break;
			}
		}

		// std::sort(result.begin(), result.end());
		return result;
    }
    
    
        /**
     * generate the neighbor words according to the word
     * to change the char from a-z
     * @param word the currecnt word to generate neighor words
     * @param visited the visited word
     * @param lexicon all the words
     * @return the neighors of the word
     */
    auto neighbor(std::string& word,
                  unordered_set<std::string>& visited,
                  unordered_set<std::string>& lexicon) -> unordered_set<std::string> {
        // contains all the neighbor words
        auto words = unordered_set<std::string>{};

        for (auto i = std::vector<int>::size_type{0}; i < word.size(); i++) {
            // because we does n
            // ot want to change the current word
            // so we use curr_char to store the char which will be replaced
            char curr_char = word[i];
            // change the curr with a-z and check whether visited and whether in lexicon
            for (auto c = 'a'; c <= 'z'; ++c) {
                word[i] = c;
                // to avoid same word
                if (curr_char == c) {
                    continue;
                }

                // the word should not visited and should in lexicon
                if (not visited.count(word) 
                   and lexicon.count(word)) {
                    words.insert(word);
                }
            }

            word[i] = curr_char;
        }

        return words;
    }

    /**
     * iterate the result according to the end word
     * it can be a recurrsion, but I do not want to do that
     * recurrsion not stable.
     *
     * In the hashmap the word stored as: end->{w1, w2, w2}
     * this means w1, w2, w3 can convert to end, similar idea for start
     *
     * @param words is the result which contain as list
     * @param word_maps hashmap, key is word, values is word ladders
     * @return a new list of result and then loop again
     */
    auto generate_result(std::vector<std::vector<std::string>>& words,
                         std::unordered_map<std::string, std::vector<std::string>>& word_maps)
       -> std::vector<std::vector<std::string>> {
        auto temp = std::vector<std::vector<std::string>>{};

        // the word_maps were stored as a hashmap
        // key is the word, value is the word ladders
        for (auto const& word_list : words) {
            auto t = word_list[0];

            // get the related word from hashmap
            for (auto const& word : word_maps[t]) {
                // insert the value in the front of list
                auto w = word_list;
                w.insert(w.begin(), word);
                temp.push_back(w);
            }
        }

        return temp;
    }
};
```

