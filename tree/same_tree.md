# Same Tree

> Given two binary trees, write a function to check if they are equal or not.
Two binary trees are considered equal if they are structurally identical and the nodes have the same values.


题目翻译:
给两棵树，写一个函数来判断这两棵树是否相同. 我们判定一棵树是否相同的条件为这两棵树的结构相同，并且每个节点的值相同.


解题思路:
这道题中规中矩，很简单，我们直接用DFS前序遍历这两棵树就可以了.

时间复杂度分析:
因为是DFS, 所以时间复杂度为O(n)


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
    bool isSameTree(TreeNode *p, TreeNode *q) {
        if(p == NULL && q == NULL)
            return true;
        else if(p == NULL || q == NULL)
            return false;
        if(p->val == q->val)
        {
            bool left = isSameTree(p->left, q->left);
            bool right = isSameTree(p->right,q->right);
            return left&&right;
        }
        return false;
    }
};

```
