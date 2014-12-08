# Flatten Binary Tree to Linked List

> Given a binary tree, flatten it to a linked list in-place.

> For example,
Given

>```
         1
        / \
       2   5
      / \   \
     3   4   6
```

> The flattened tree should look like:
>```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

给定一颗二叉树，将其扁平化处理，我们可以看到处理之后的节点顺序其实跟前序遍历原二叉树的一致，所以我们只需要前序遍历二叉树同时处理就可以了。代码如下：

```
class Solution {
public:
    void flatten(TreeNode *root) {
                if(!root) {
            return;
        }

        vector<TreeNode*> ns;
        TreeNode dummy(0);

        TreeNode* n = &dummy;

        ns.push_back(root);

        while(!ns.empty()) {
            TreeNode* p = ns.back();
            ns.pop_back();

            //挂载到右子树
            n->right = p;
            n = p;


            //右子树压栈
            if(p->right) {
                ns.push_back(p->right);
                p->right = NULL;
            }

            //左子树压栈
            if(p->left) {
                ns.push_back(p->left);
                p->left = NULL;
            }
        }
    }
};
```
