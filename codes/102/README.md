# 102. Binary Tree Level Order Traversal

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2:**

```
Input: root = [1]
Output: [[1]]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

## Solution

`BFS`就能解决。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        std::queue<TreeNode*> queue;
        // std::queue<TreeNode*> nq;

        auto res = vector<vector<int>>();

        if (root == NULL) return res;
        int cnt = 1;
        int cntn = 0;

        queue.push(root);
        auto curr = vector<int>();


        while (!queue.empty()) {
            auto node = queue.front();
            queue.pop();
            cnt --;

            curr.push_back(node->val);

            if (node->left != NULL) {
                queue.push(node->left);
                cntn ++;
            }
            if (node->right != NULL) {
                queue.push(node->right);
                cntn ++;
            }

            if (cnt == 0) {
                res.push_back(curr);
                curr.clear();
                
                cnt = cntn;
                cntn = 0;
            }

        }

        return res;
    }
};
```

