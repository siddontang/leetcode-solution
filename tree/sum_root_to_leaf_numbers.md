# Sum Root to Leaf Numbers

> Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.
> An example is the root-to-leaf path `1->2->3` which represents the number `123`.
> Find the total sum of all root-to-leaf numbers.
> For example,

```
    1
   / \
  2   3
```
> The root-to-leaf path `1->2` represents the number `12`.
> The root-to-leaf path `1->3` represents the number `13`.
> Return the sum = 12 + 13 = 25.

题目翻译：
给定一棵二叉树，仅包含0到9这些数字，每一条从根节点到叶节点的路径表示一个数。例如，路径`1->2->3`表示数值123。求出所有路径表示的数值的和。
上述例子中，路径`1->2`表示数值12，路径`1->3`表示数值13。它们的和是25。

题目分析:
从根节点到叶节点的遍历方法是深度优先搜索(DFS)。解决本题只需在遍历过程中记录路径中的数字，在到达叶节点的时候把记录下来的数字转换成数值，加到和里面即可。

时间复杂度:
O(n)

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
    int sumNumbers(TreeNode* root) {
        vector<int> arr;
        int sum = 0;
        dfs(root, arr, sum);
        return sum;
    }
    
    int vec2num(vector<int>& vec) {
        int num = 0;
        for (auto n : vec) {
            num = num * 10 + n;
        }
        return num;
    }
    
    void dfs(TreeNode* node, vector<int>& arr, int& sum) {
        if (node == nullptr) return;
        arr.push_back(node->val);
        if (node->left == nullptr && node->right == nullptr) {
            sum += vec2num(arr);
        } else {
            if (node->left != nullptr) dfs(node->left, arr, sum);
            if (node->right != nullptr) dfs(node->right, arr, sum);
        }
        arr.pop_back();
    }
    
};
```


