# Binary Search Tree Iterator

> Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

> Calling next() will return the next smallest number in the BST.

> Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

题目翻译:
实现二叉查找树的迭代器。你实现的迭代器会使用二叉查找树的根节点来初始化。

调用next()方法会返回二叉搜索树里下一个最小的数。

注意：next()和hasNext()方法的时间和空间复杂度要求分别是O(1)和O(h)，其中h是树的高度。

解题思路:
二叉查找树的性质是，每个节点的左节点小于它，而右节点大于它。最小的节点总是存在于树的最左侧，直接遍历获取的时间复杂度是O(h)，而空间复杂度是O(1)。

题目中要求实现O(1)的时间复杂度和O(h)的空间复杂度。注意到每次获取最左侧的节点时，我们总会重复访问从根节点开始的所有左节点。因此我们可以用栈来存储遍历时经过的节点。

初始化的时候，把从根节点开始的所有左节点压入栈中，此时栈顶元素是最左下方的节点，亦即当前的最小值。next()函数弹出并返回栈顶元素。然后将弹出的节点的右子树的左侧节点依次压入栈中。这个操作和初始化时的压栈操作是相同的，故将其单独提取为一个函数。使用了栈以后，hasNext()函数只需返回栈是否为空即可。若栈不为空，那么尚有最小的元素未被访问，否则整棵树都已经被访问过。

时间和空间复杂度:
如题目要求，时间复杂度是O(1), 空间复杂度是O(h)。

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
class BSTIterator {
public:
    BSTIterator(TreeNode *root) {
        pushStack(root);
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !st.empty();
    }

    /** @return the next smallest number */
    int next() {
        TreeNode* node = st.top();
        st.pop();
        pushStack(node->right);
        return node->val;
    }
private:
    stack<TreeNode*> st;
    void pushStack(TreeNode *node) {
        while (node != nullptr) {
            st.push(node);
            node = node->left;
        }
    }
};

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = BSTIterator(root);
 * while (i.hasNext()) cout << i.next();
 */
```
