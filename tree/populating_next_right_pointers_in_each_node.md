# Populating Next Right Pointers in Each Node

> Given a binary tree

>```
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```
> Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

> Initially, all next pointers are set to NULL.

> Note:

> You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
For example,
Given the following perfect binary tree,
```
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
```
After calling your function, the tree should look like:
```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```

这题需要在一棵完全二叉树中使用next指针连接旁边的节点，我们可以发现一些规律。

+ 如果一个子节点是根节点的左子树，那么它的next就是该根节点的右子树，譬如上面例子中的4，它的next就是2的右子树5。
+ 如果一个子节点是根节点的右子树，那么它的next就是该根节点next节点的左子树。譬如上面的5，它的next就是2的next（也就是3）的左子树。

所以代码如下：

```c++
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if(!root) {
            return;
        }

        TreeLinkNode* p = root;
        TreeLinkNode* first = NULL;
        while(p) {
            //记录下层第一个左子树
            if(!first) {
                first = p->left;
            }
            //如果有左子树，那么next就是父节点
            if(p->left) {
                p->left->next = p->right;
            } else {
                //叶子节点了，遍历结束
                break;
            }

            //如果有next，那么设置右子树的next
            if(p->next) {
                p->right->next = p->next->left;
                p = p->next;
                continue;
            } else {
                //转到下一层
                p = first;
                first = NULL;
            }
        }
    }
};
```

# Populating Next Right Pointers in Each Node II

> What if the given tree could be any binary tree? Would your previous solution still work?

> Note:

> You may only use constant extra space.
For example,
Given the following binary tree,
```
         1
       /  \
      2    3
     / \    \
    4   5    7
```
After calling your function, the tree should look like:
```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```

不同于上一题，这题的二叉树并不是完全二叉树，我们不光需要提供first指针用来表示一层的第一个元素，同时也需要使用另一个lst指针表示该层上一次遍历的元素。那么我们只需要处理好如何设置last的next指针就可以了。

代码如下:

```c++
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if(!root) {
            return;
        }

        TreeLinkNode* p = root;
        TreeLinkNode* first = NULL;
        TreeLinkNode* last = NULL;

        while(p) {
            //设置下层第一个元素
            if(!first) {
                if(p->left) {
                    first = p->left;
                } else if(p->right) {
                    first = p->right;
                }
            }

            if(p->left) {
                //如果有last，则设置last的next
                if(last) {
                    last->next = p->left;
                }
                //last为left
                last = p->left;
            }

            if(p->right) {
                //如果有last，则设置last的next
                if(last) {
                    last->next = p->right;
                }
                //last为right
                last = p->right;
            }

            //如果有next，则转到next
            if(p->next) {
                p = p->next;
            } else {
                //转到下一层
                p = first;
                last = NULL;
                first = NULL;
            }
        }
    }
};
```

其实我们可以看到，第一题只是第二题的特例，所以第二题的解法也同样适用于第一题。
