# Recover Binary Search Tree

> Two elements of a binary search tree (BST) are swapped by mistake.

> Recover the tree without changing its structure.

> Note:

> A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

这题需要修复一颗二叉搜索树的两个交换节点数据，我们知道对于一颗二叉搜索树来说，如果按照中序遍历，那么它输出的值是递增有序的，所以我们只需要按照中序遍历输出，在输出结果里面找到两个异常数据（比它后面输出结果大），交换这两个节点的数据就可以了。

但是这题要求使用O(1)的空间，如果采用通常的中序遍历（递归或者栈）的方式，都需要O(N)的空间，所以这里我们用Morris Traversal的方式来进行树的中序遍历。

Morris Traversal中序遍历的原理比较简单：

+ 如果当前节点的左孩子为空，则输出当前节点并将其有孩子作为当前节点。
+ 如果当前节点的左孩子不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点，也就是当前节点左子树的最右边的那个节点。
    + 如果前驱节点的右孩子为空，则将它的右孩子设置为当前节点，当前节点更新为其左孩子。
    + 如果前驱节点的右孩子为当前节点，则将前驱节点的右孩子设为空，输出当前节点，当前节点更新为其右孩子。

重复上述过程，直到当前节点为空，递归的时候我们同时需要记录错误的节点。那么我们如何知道一个节点的数据是不是有问题呢？对于中序遍历来说，假设当前节点为cur，它的前驱节点为pre，如果cur的值小于pre的值，那么cur和pre里面的数据就是交换的了。

代码如下：

```c++
class Solution {
public:
    void recoverTree(TreeNode *root) {
        TreeNode* cur = 0;
        TreeNode* pre = 0;
        TreeNode* p1 = 0;
        TreeNode* p2 = 0;
        TreeNode* preCur = 0;

        bool found = false;

        if(!root) {
            return;
        }

        cur = root;
        while(cur) {
            if(!cur->left) {
                //记录p1和p2
                if(preCur && preCur->val > cur->val) {
                    if(!found) {
                        p1 = preCur;
                        found = true;
                    }
                    p2 = cur;
                }

                preCur = cur;
                cur = cur->right;
            } else {
                pre = cur->left;
                while(pre->right && pre->right != cur) {
                    pre = pre->right;
                }

                if(!pre->right) {
                    pre->right = cur;
                    cur = cur->left;
                } else {
                    //记录p1和p2
                    if(preCur->val > cur->val) {
                        if(!found) {
                            p1 = preCur;
                            found = true;
                        }
                        p2 = cur;
                    }
                    preCur = cur;
                    pre->right = NULL;
                    cur = cur->right;
                }
            }
        }

        if(p1 && p2) {
            int t = p1->val;
            p1->val = p2->val;
            p2->val = t;
        }
    }
};
```
