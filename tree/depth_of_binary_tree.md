# Maximum Depth of Binary Tree

> Given a binary tree, find its maximum depth.

> The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

这题要求我们求出一个二叉树最大深度，也就是从根节点到最远的叶子节点的距离。

对于这题，我们只需要递归遍历二叉树，达到一个叶子节点的时候，记录深度，我们就能得到最深的深度了。

代码如下：

```c++
class Solution {
public:
    int num;
    int maxDepth(TreeNode *root) {
        if(!root) {
            return 0;
        }

        //首先初始化num为最小值
        num = numeric_limits<int>::min();
        travel(root, 1);
        return num;
    }

    void travel(TreeNode* node, int level) {
        //如果没有左子树以及右子树了，就到了叶子节点
        if(!node->left && !node->right) {
            num = max(num, level);
            return;
        }

        if(node->left) {
            travel(node->left, level + 1);
        }

        if(node->right) {
            travel(node->right, level + 1);
        }
    }
};
```

# Minimum Depth of Binary Tree

> Given a binary tree, find its minimum depth.

> The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

这题跟上题几乎一样，区别在于需要求出根节点到最近的叶子节点的深度，我们仍然使用遍历的方式。

代码如下：

```c++
class Solution {
public:
    int n;
    int minDepth(TreeNode *root) {
        if(!root) {
            return 0;
        }

        //初始化成最大值
        n = numeric_limits<int>::max();
        int d = 1;

        depth(root, d);
        return n;
    }

    void depth(TreeNode* node, int& d) {
        //叶子节点，比较
        if(!node->left && !node->right) {
            n = min(n, d);
            return;
        }

        if(node->left) {
            d++;
            depth(node->left, d);
            d--;
        }

        if(node->right) {
            d++;
            depth(node->right, d);
            d--;
        }
    }
};
```
