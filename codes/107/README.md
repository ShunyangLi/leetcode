# 107. Binary Tree Level Order Traversal II

Given the `root` of a binary tree, return *the bottom-up level order traversal of its nodes' values*. (i.e., from left to right, level by level from leaf to root).

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[15,7],[9,20],[3]]
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

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`

## Solution

和102题思路差不多，可以bfs遍历之后再reverse一下数组就行。

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
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

        std::reverse(res.begin(), res.end());

        return res;
    }
};
```

