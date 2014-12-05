# Path Sum II

> Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

> For example:
Given the below binary tree and sum = 22.


```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

```

> return

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```
题目翻译：
给定一个二叉树，并且给定一个值，找出所有从根节点到叶子节点和等于这个给定值的路径.上面的例子可以很好地让读者理解这个题目的目的.

解题思路:
这个题目和Path Sum的解法几乎是一模一样，都是用dfs来进行求解，不过就是在传参数的时候有些不同了，因为题目的要求也不同.

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
    vector<vector<int> > pathSum(TreeNode *root, int sum) {
        vector<vector<int>> ret;
        if(root == NULL)
            return ret;
        vector<int> curr;
        DFS(ret,curr,sum,0,root);
        return ret;
    }

    void DFS(vector<vector<int>>& ret, vector<int> curr, int sum, int tmpsum, TreeNode* root)
    {
        if(root == NULL)
            return;
        tmpsum+=root->val;
        curr.push_back(root->val);
        if(tmpsum == sum)
        {
            if(root->left == NULL&&root->right == NULL)
            {
                ret.push_back(curr);
                return;
            }
        }
        DFS(ret,curr,sum,tmpsum,root->left);
        DFS(ret,curr,sum,tmpsum,root->right);
    }
};
```


