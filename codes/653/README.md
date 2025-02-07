# 653. Two Sum IV - Input is a BST

Given the `root` of a binary search tree and an integer `k`, return `true` *if there exist two elements in the BST such that their sum is equal to* `k`, *or* `false` *otherwise*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_1.jpg)

```
Input: root = [5,3,6,2,4,null,7], k = 9
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_2.jpg)

```
Input: root = [5,3,6,2,4,null,7], k = 28
Output: false
```

## Solution

可以先用in-order遍历一下bst拿到从小到大的元素，然后用双指针

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
    void inorderTraversal(TreeNode* root, vector<int>& vals) {
        if (root == nullptr) return;

        inorderTraversal(root->left, vals);
        vals.push_back(root->val);
        inorderTraversal(root->right, vals);
    }

    bool findTarget(TreeNode* root, int k) {
        auto numbers = vector<int>();

        inorderTraversal(root, numbers);

        int l = 0, r = numbers.size() - 1;

        while (l < r) {
            if (numbers[l] + numbers[r] == k) {
                return true;
            } else if (numbers[l] + numbers[r] > k) {
                r --;
            } else {
                l ++;
            }
        }

        return false;


    }
};
```
