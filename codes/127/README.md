
# 126. Word Ladder


Given two words (*beginWord* and *endWord*), and a dictionary's word list, find the length of shortest transformation sequence from *beginWord* to *endWord*, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list.

**Note:**

- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume *beginWord* and *endWord* are non-empty and are not the same.

**Example 1:**

```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

词语接龙，一个老外很喜欢的游戏，我觉得他们的test有问题。。。这个数量就很奇怪，不过我还是按照我的方法来，我不觉得我写错了，使用了一个bfs的算法，因为每次改变一个单词其中的一个字符，然后判断新生成的字符是否在wordList之间，然后再判断是不是已经visited过的。

```java
import java.util.*;

public class WordLadder {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        int step = 0;
        if (!wordList.contains(endWord)) return step;
        Queue<String> queue = new LinkedList<>();
        queue.offer(beginWord);
        Set<String> visited = new HashSet<>();

        while (!queue.isEmpty()) {
            ++step;
            for (int x = 0; x < queue.size(); x ++) {
                String str = queue.poll();
                visited.add(str);
                if (str.equals(endWord)) return step;
                for (int i = 0; i < str.length(); i ++) {
                    char[] temp = str.toCharArray();
                    char ch = temp[i];
                    for (char c = 'a'; c <= 'z'; c ++) {
                        if (c == ch) continue;
                        temp[i] = c;
                        String t = new String(temp);
                        if (!wordList.contains(t)) continue;
                        if (visited.contains(t)) continue;
                        queue.offer(t);
                        visited.add(t);
                    }
                }
            }
        }
        return step;
    }


    public static void main(String[] args) {
        WordLadder w = new WordLadder();
        List<String> list = new ArrayList<String>(
                Arrays.asList("hot","dot","dog","lot","log","cog")
        );

        int res = w.ladderLength("hit", "cog", list);

        System.out.println(res);

    }
}
```
