# Binary Tree Path

> Given a binary tree, return all root-to-leaf paths.

> For example, given the following binary tree:
```
   1
 /   \
2     3
 \
  5
```
> All root-to-leaf paths are:
```
["1->2->5", "1->3"]
```

题目翻译：
给定一棵二叉树，返回所有从根节点到叶节点的路径。

题目分析：
本题属于二叉树的遍历问题，可以用深度优先搜索来解决。

使用栈来记录遍历过程中访问过的节点。递归地访问每个节点的子节点，如果遇到叶节点，则输出记录的路径。返回上一层之前弹出栈顶元素。
C++的vector容器也能做到后进先出，所以下面的代码并没有使用std::stack类来实现。

生成输出的字符串时，可以使用std::stringstream类来完成，类似于Java和C#中的StringBuilder。

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
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        if (root == nullptr) return result;
        vector<int> path;
        bfs(root, path, result);
        return result;
    }

private:
	// 递归函数，深度优先搜索
    void bfs(TreeNode* node, vector<int>& path, vector<string>& result) {
        if (node == nullptr) return;
        path.push_back(node->val);
        if (node->left == nullptr && node->right == nullptr)
            result.push_back(generatePath(path));
        else {
            if (node->left != nullptr) {
                bfs(node->left, path, result);
                path.pop_back();
            }
            if (node->right != nullptr) {
                bfs(node->right, path, result);
                path.pop_back();
            }
        }
    }
	
	// 辅助函数，用于生成路径字符串
    string generatePath(vector<int> path) {
        stringstream ss;
        int i;
        for (i = 0; i < path.size() - 1; i++) ss << path[i] << "->";
        ss << path[i];
        return ss.str();
    }
};
```
