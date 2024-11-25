
# 111. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example:**

Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its minimum depth = 2.

关于tree的算法可以去看我另一篇文章https://shunyangli.github.io/2020/05/03/Algorithm/#more

```java
public class MinimumDepthofBinaryTree {
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        if (root.left == null && root.right == null) return 1;

        int left = minDepth(root.left);
        int right = minDepth(root.right);

        if (root.left == null) return 1 + right;
        if (root.right == null) return 1 + left;

        return Math.min(left, right) + 1;
    }
}
```