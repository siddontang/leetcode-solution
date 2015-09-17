# Count Complete Tree Nodes 

> Given a complete binary tree, count the number of nodes.

> Definition of a complete binary tree from Wikipedia:
> In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.


题目翻译:
给定一棵完全二叉树，统计树中节点的个数。

完全二叉树在维基百科上的定义是：除了最后一层，完全二叉树的每一层的节点都是满的。最后一层的节点全都靠近左侧。最后一层的深度为h时，这一层可以有1到2h个节点。


解题思路:
其实很多文献上对完全二叉树的定义都是从满二叉树延伸开来的。满二叉树就是深度为k并且有2^k-1个节点的二叉树。而完全二叉树可以直观地理解为从右下方水平方向上连续地移除一部分叶节点所产生的二叉树。满二叉树本身也是完全二叉树。

对于一棵满二叉树，其最左侧的叶节点的深度必然与最右侧的叶节点的深度相同。若不同，那么我们就递归地检查当前结点的左右子树，分别判断其最左和最右叶节点的深度是否相同。不难发现，对于完全二叉树，总能找到从某个节点开始下面的子树都是满二叉树。从而我们可以利用满二叉树的性质计算其节点个数。当递归函数返回时，左右子树的节点个数之和再加上1即为以当前结点作为根节点的总的节点个数。

注：计算2的n次方用移位运算效率最高。n为正整数则左移，n为负整数则右移。

代码如下:
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) {
        int depthLeft = leftDepth(root);
        int depthRight = rightDepth(root);
        if (depthLeft == depthRight) return (1 << depthLeft) - 1;
        return countNodes(root->left) + countNodes(root->right) + 1;
    }
private:
    int leftDepth(TreeNode* node) {
        int depth = 0;
        while (node != nullptr) {
            depth++;
            node = node->left;
        }
        return depth;
    }
    
    int rightDepth(TreeNode* node) {
        int depth = 0;
        while (node != nullptr) {
            depth++;
            node = node->right;
        }
        return depth;
    }
};

```
