# Validate Binary Search Tree

> Given a binary tree, determine if it is a valid binary search tree (BST).

> Assume a BST is defined as follows:

> + The left subtree of a node contains only nodes with keys less than the node's key.
+ The right subtree of a node contains only nodes with keys greater than the node's key.
+ Both the left and right subtrees must also be binary search trees.

这题需要判断是不是一个正确的二叉搜索树，比较简单地一道题。

我们通过递归整棵树来解决，代码如下：

```c++
class Solution {
public:
    bool isValidBST(TreeNode *root) {
        return valid(root, numeric_limits<int>::min(), numeric_limits<int>::max());
    }

    bool valid(TreeNode* node, int minVal, int maxVal) {
        if(!node) {
            return true;
        }

        if(node->val <= minVal || node->val >= maxVal) {
            return false;
        }

        return valid(node->left, minVal, node->val) &&
        valid(node->right, node->val, maxVal);
    }
};
```
