# 515. Find Largest Value in Each Tree Row

Given the `root` of a binary tree, return *an array of the largest value in each row* of the tree **(0-indexed)**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg)

```
Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]
```

**Example 2:**

```
Input: root = [1,2,3]
Output: [1,3]
```

 

**Constraints:**

- The number of nodes in the tree will be in the range `[0, 104]`.
- `-231 <= Node.val <= 231 - 1`

## Solution

`bfs`遍历一遍就行

```c++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        auto res = vector<int>();

        if (root == NULL) return res;

        auto currs = vector<TreeNode*>();
        auto nexts = vector<TreeNode*>();

        currs.push_back(root);

        while (!currs.empty()) {
            int max_val = INT_MIN;

            for (auto const& n : currs) {
                if (n->val > max_val) max_val = n->val;
                if (n->left != NULL) nexts.push_back(n->left);
                if (n->right != NULL) nexts.push_back(n->right);
            }

            currs = nexts;
            nexts.clear();
            res.push_back(max_val);
        }

        return res;
    }
};
```

