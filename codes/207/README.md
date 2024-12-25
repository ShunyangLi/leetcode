# 207. Course Schedule

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

## Solution

可以通过`bfs`来判断是否有环就行了，但`bfs`判断的话就是`toposort`了。

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        if (prerequisites.empty()) return true;

        int marked = 0;
        auto in_degree = vector<int>(numCourses, 0);
        auto dg = vector<vector<int>>(numCourses);

        for (auto const& pre : prerequisites) {
            dg[pre[1]].push_back(pre[0]);
            in_degree[pre[0]] += 1;
        }

        queue<int> q;

        for (int i = 0; i < in_degree.size(); i ++) {
            if(in_degree[i] == 0) q.push(i);
        }


        while (!q.empty()) {
            auto u = q.front();
            q.pop();

            marked += 1;

            for (auto const& v : dg[u]) {
                in_degree[v] -- ;

                if (in_degree[v] == 0) q.push(v);
            }
        }

        return marked  == numCourses;
    }
};
```

