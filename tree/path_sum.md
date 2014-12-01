# Path Sum

> Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

> For example:
Given the below binary tree and sum = 22,

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

```
> return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

题目翻译:
给定一颗二叉树和一个特定值，写一个方法来判定这棵树是否存在这样一种条件，使得从root到其中一个叶子节点的路径的和等于给定的sum值.

解题思路:
这道题很常规，直接用DFS就可以求解.

时间复杂度:
O(n)

代码如下:
```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode *root, int sum) {
        if(root == NULL)
            return false;
        return DFS(sum, 0, root);
    }

    bool DFS(int target, int sum, TreeNode* root)
    {
        if(root == NULL)
            return false;
        sum += root->val;
        if(root->left == NULL && root->right == NULL)
        {
            if(sum == target)
                return true;
            else
                return false;
        }
        bool leftPart = DFS(target, sum, root->left);
        bool rightPart = DFS(target, sum, root->right);
        return leftPart||rightPart;
    }

};

```
