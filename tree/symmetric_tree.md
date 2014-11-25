# Symmetric Tree

> Given a binary tree, check whether it is a mirror of itself(ie, symmetric around its center)

> For example, this tree is symmetric:


```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
> But the following tree is not.

```
    1
   / \
  2   2
   \   \
   3    3
```

题目翻译：
判断一棵树是不是自己的镜像， 根据以上正反两个例子，我想大家都明白这道题的题目要求了，简单明了.

解题思路:
递归:
这道题没什么特别的地方，现在这里简单的分析一下解题思路，从根节点往下，我们要判断三个条件.
1. 左右两个节点的大小是否相同.
2. 左节点的左孩子是否和右节点的右孩子相同.
3. 左节点的右孩子是否和右节点的左孩子相同.


循环:
这道题的难点在于循环解法， 如果是循环解法，我们必须要用到额外的存储空间用于回溯，关键是对于这道题目， 我们要用多少？要怎么用？要用什么样的存储空间？



递归求解，如果以上三个条件对于每一层都满足，我们就可以认为这棵树是镜像树.

时间复杂度:
递归:本质其实就是DFS,时间复杂度为O(n),空间复杂度O(1)
递归:时间复杂度O(n),空间复杂度O(n)


递归代码如下:

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
    bool isSymmetric(TreeNode *root) {
        if(root == NULL)
            return true;
        return Helper(root->left,root->right);
    }

    bool Helper(TreeNode* left, TreeNode* right)
    {
        if(left == NULL&&right == NULL)
            return true;
        else if(left == NULL||right == NULL)
            return false;
        bool cond1 = left->val == right->val;
        bool cond2 = Helper(left->left,right->right);
        bool cond3 = Helper(left->right, right->left);
        return cond1&&cond2&&cond3;
    }

};
```

循环解法:
我们主要想介绍一下这道题的循环解法，对于循环，我们要满足对于每一层进行check，代替了递归，一般树的循环遍历，我们都是用FIFO的queue来作为临时空间存储变量的，所以这道题我们也选取了queue，但是我们用两个queue，因为我们要对于左右同时进行检查，很显然一个queue是不够的，具体实现细节，咱们还是看代码吧，，我相信代码更能解释方法.

循环代码如下:
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
    bool isSymmetric(TreeNode *root) {
       if(root == NULL)
            return true;
        TreeNode* n1 = root->left;
        TreeNode* n2 = root->right;
        if(!n1&&!n2)
            return true;
        if((!n1&&n2)||(n1&&!n2))
            return false;
        queue<TreeNode*> Q1;
        queue<TreeNode*> Q2;
        Q1.push(n1);
        Q2.push(n2);
        while(!Q1.empty() && !Q2.empty())
        {
            TreeNode* tmp1 = Q1.front();
            TreeNode* tmp2 = Q2.front();
            Q1.pop();
            Q2.pop();
            if((!tmp1&&tmp2) || (tmp1&&!tmp2))
                return false;
            if(tmp1&&tmp2)
            {
                if(tmp1->val != tmp2->val)
                    return false;
                Q1.push(tmp1->left);
                Q1.push(tmp1->right); //note: this line we should put the mirror sequence in two queues
                Q2.push(tmp2->right);
                Q2.push(tmp2->left);
            }
        }
        return true;
    }
};
```
