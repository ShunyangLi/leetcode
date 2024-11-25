
# 144. Binary Tree Preorder Traversal

Given a binary tree, return the *preorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

就是一个前序的遍历：根左右

```java
import java.util.LinkedList;
import java.util.List;

public class BinaryTreePreorderTraversal {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();

        traversal(root, res);
        return res;

    }

    public void traversal(TreeNode root, List<Integer> res) {
        if (root == null) return;

        res.add(root.val);
        traversal(root.left, res);
        traversal(root.right, res);
    }
}
```