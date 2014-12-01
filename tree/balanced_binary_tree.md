# Balanced Binary Tree

> Given a binary tree, determine if it is height-balanced.

> For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

题目翻译:
给定一颗二叉树， 写一个函数来检测这棵树是否是平衡二叉树. 对于这个问题, 一颗平衡树的定义是其中任意节点的左右子树的高度差不大于1.

解题思路:
这道题其实就是应用DFS，对于一颗二叉树边计算树的高度边计算差值，针对树里面的每一个节点计算它的左右子树的高度差，如果差值大于1，那么就返回-1，如果不大于1，从下往上再次检测.

时间复杂度:
由于是运用DFS，所以时间复杂度为O(n).

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
    bool isBalanced(TreeNode *root) {
        //corner case check
        if(root == NULL)
            return true;
        int isBalanced = getHeight(root);
        if(isBalanced != -1)
            return true;
        else
            return false;
    }

    int getHeight(TreeNode* root)
    {
        if(root == NULL)
            return 0;
        int leftHeight = getHeight(root->left);
        if(leftHeight == -1)
            return -1;
        int rightHeight = getHeight(root->right);
        if(rightHeight == -1)
            return -1;
        int diffHeight = rightHeight > leftHeight? rightHeight-leftHeight:leftHeight-rightHeight;
        if(diffHeight > 1)
            return -1;
        else
            return diffHeight = (rightHeight>leftHeight?rightHeight:leftHeight)+1;
    }
};
```
