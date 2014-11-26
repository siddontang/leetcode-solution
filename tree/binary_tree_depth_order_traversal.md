# Binary Tree Depth Order Traversal

前面我们解决了tree的level order遍历问题，这里我们需要来处理tree的depth order，也就是前序，中序和后序遍历。

# Binary Tree Preorder Traversal

> Given a binary tree, return the preorder traversal of its nodes' values.

> For example:
> Given binary tree {1,#,2,3},

>```
   1
    \
     2
    /
   3
```

> return [1,2,3].

> Note: Recursive solution is trivial, could you do it iteratively?

给定一颗二叉树，使用迭代的方式进行前序遍历。

因为不能递归，所以我们只能使用stack来保存迭代状态。

对于前序遍历，根节点是最先访问的，然后是左子树，最后才是右子树。当访问到根节点的时候，我们需要将右子树压栈，这样访问左子树之后，才能正确地找到对应的右子树。

代码如下：

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> vals;
        if(root == NULL) {
            return vals;
        }

        vector<TreeNode*> nodes;

        //首先将root压栈
        nodes.push_back(root);

        while(!nodes.empty()) {
            TreeNode* n = nodes.back();
            vals.push_back(n->val);

            //访问了该节点，出栈
            nodes.pop_back();

            //如果有右子树，压栈保存
            if(n->right) {
                nodes.push_back(n->right);
            }

            //如果有左子树，压栈保存
            if(n->left) {
                nodes.push_back(n->left);
            }
        }

        return vals;
    }
};
```

# Binary Tree Inorder Traversal

给定一颗二叉树，使用迭代的方式进行中序遍历。

对于中序遍历，首先遍历左子树，然后是根节点，最后才是右子树，所以我们需要用stack记录每次遍历的根节点，当左子树遍历完成之后，从stack弹出根节点，得到其右子树，开始新的遍历。

代码如下：

```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode *root) {
        vector<int> vals;
        if(root == NULL) {
            return vals;
        }

        vector<TreeNode*> nodes;
        TreeNode* p = root;
        while(p || !nodes.empty()) {
            //这里一直遍历左子树，将根节点压栈
            while(p) {
                nodes.push_back(p);
                p = p->left;
            }

            if(!nodes.empty()) {
                p = nodes.back();
                vals.push_back(p->val);

                //将根节点弹出，获取右子树
                nodes.pop_back();
                p = p->right;
            }
        }

        return vals;
    }
};
```

# Binary Tree Postorder Traversal

给定一颗二叉树，使用迭代的方式进行后序遍历。

对于后序遍历，首先遍历左子树，然后是右子树，最后才是根节点。当遍历到一个节点的时候，首先我们将右子树压栈，然后将左子树压栈。这里需要注意一下出栈的规则，对于叶子节点来说，直接可以出栈，但是对于根节点来说，我们需要一个变量记录上一次出栈的节点，如果上一次出栈的节点是该根节点的左子树或者右子树，那么该根节点可以出栈，否则这个根节点是新访问的节点，将右和左子树分别压栈。

代码如下：

```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> vals;
        if(root == NULL) {
            return vals;
        }

        vector<TreeNode*> nodes;
        TreeNode* pre = NULL;

        nodes.push_back(root);

        while(!nodes.empty()) {
            TreeNode* p = nodes.back();
            //如果不判断pre，我们就没法正确地出栈了
            if((p->left == NULL && p->right == NULL) ||
                (pre != NULL && (pre == p->left || pre == p->right))) {
                vals.push_back(p->val);
                nodes.pop_back();
                pre = p;
            } else {
                //右子树压栈
                if(p->right != NULL) {
                    nodes.push_back(p->right);
                }

                //左子树压栈
                if(p->left != NULL) {
                    nodes.push_back(p->left);
                }
            }
        }

        return vals;
    }
};
```

## 总结

可以看到，树的遍历通过递归或者堆栈的方式都是比较容易的，网上还有更牛的不用栈的方法，只是我没理解，就不做过多说明了。
