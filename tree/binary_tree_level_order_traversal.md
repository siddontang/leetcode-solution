# Binary Tree Level Order Traversal

> Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

> For example:
Given binary tree {3,9,20,#,#,15,7},

```
    3
   / \
  9  20
    /  \
   15   7
```
> return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

题目翻译:
给定一颗二叉树，返回一个二维数组，使这个二维数组的每一个元素代表着二叉树的一层的元素.例子已经明确给出.

题目分析:
对于二叉树的问题，我们第一想到的就是DFS或者BFS, DFS更易于理解代码，如果处理数据量不是很大的话.对于这样的面试题，我建议用DFS来求解.

需要注意的点为:
1. 对于一棵树，如果我们要求每一层的节点，并且存在一个二维数组里，首先我们要建一个二维数组，但是这个二维数组建多大的合适呢？我们就要求出这颗树的深度，根据深度来建立二维数组.
2. 题目要求为从左往右添加,所以我们也就是要先放左边的节点，再放右边的节点.
3. 对于这道题，我们首先就是要用DFS来求出这颗树的高度，之后再用DFS对于每一层遍历，这样节省了空间复杂度.

时间复杂度分析:
由于两次DFS是并列的，并没有嵌套，所以我们的时间复杂度为O(n).

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
/* for this question, we need to construct the ret vector first
   thus, we need to know the depth of this tree, we write a simple
   function to calculate the height of this tree */
    vector<vector<int> > levelOrder(TreeNode *root) {
       int depth = getHeight(root);
       vector<vector<int>> ret(depth);
       if(depth == 0) //invalid check
            return ret;
        getSolution(ret,root,0);
        return ret;
    }

    void getSolution(vector<vector<int>>& ret, TreeNode* root, int level)
    {
        if(root == NULL)
            return;
        ret[level].push_back(root->val);
        getSolution(ret,root->left,level+1);
        getSolution(ret,root->right,level+1);
    }

    int getHeight(TreeNode* root)
    {
        if(root == NULL)
            return 0;
        int left = getHeight(root->left);
        int right = getHeight(root->right);
        int height = (left > right?left:right)+1;
        return height;
    }
};
```

# Binary Tree Level Order Traversal II

> Given a binary tree, return the bottom-up level order traversal of its nodes' values. (from left to right, level by level from leaf to root)


> For example:
Given binary tree {3,9,20,#,#,15,7},

```
    3
   / \
  9  20
    /  \
   15   7
```
> return its level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]

```
题目翻译:
给定一颗二叉树， 返回一个二维数组，这个二维数组要满足这个条件，二维数组的第一个一维数组要是这可二叉树的最下面一层，之后以此类推，根据以上例子应该知道要求的条件。

题目分析 && 解题思路:
这道题和Binary Tree Level Order Traversal 几乎是一摸一样的，唯一不同的就是二维数组的存储顺序，详见以下代码.

时间复杂度:
O(n)-树的dfs均为O(n)

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
    vector<vector<int> > levelOrderBottom(TreeNode *root) {
        int depth = height(root);
        vector<vector<int>> ret(depth);
        if(depth == 0)
            return ret;
        DFS(ret,ret.size()-1, root);
        return ret;
    }

    void DFS(vector<vector<int>>& ret, int level, TreeNode* root)
    {
        if(root == NULL)
            return;
        ret[level].push_back(root->val);
        DFS(ret,level-1,root->left);
        DFS(ret,level-1,root->right);
    }

    /* get the height first of all */
    int height(TreeNode* root)
    {
        if(root == NULL)
            return 0;
        int left_side = height(root->left);
        int right_side = height(root->right);
        int height = (left_side > right_side?left_side:right_side)+1;
        return height;
    }
};

```
