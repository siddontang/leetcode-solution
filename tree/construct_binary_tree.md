# Construct Binary Tree from Inorder and Postorder Traversal

> Given inorder and postorder traversal of a tree, construct the binary tree.

要知道如何构建二叉树，首先我们需要知道二叉树的几种遍历方式，譬如有如下的二叉树：

```
                1
        --------|-------
        2               3
    ----|----       ----|----
    4       5       6       7
```

+ 前序遍历 1245367
+ 中序遍历 4251637
+ 后续遍历 4526731

具体到上面这一题，我们知道了一个二叉树的中序遍历以及后序遍历的结果，那么如何构建这颗二叉树呢？

仍然以上面那棵二叉树为例，我们可以发现，对于后序遍历来说，最后一个元素一定是根节点，也就是1。然后我们在中序遍历的结果里面找到1所在的位置，那么它的左半部分就是其左子树，有半部分就是其右子树。

我们将中序遍历左半部分425取出，同时发现后序遍历的结果也在相应的位置上面，只是顺序稍微不一样，也就是452。我们可以发现，后序遍历中的2就是该子树的根节点。

上面说到了左子树，对于右子树，我们取出637，同时发现后序遍历中对应的数据偏移了一格，并且顺序也不一样，为673。而3就是这颗右子树的根节点。

重复上述过程，通过后续遍历找到根节点，然后在中序遍历数据中根据根节点拆分成两个部分，同时将对应的后序遍历的数据也拆分成两个部分，重复递归，就可以得到整个二叉树了。

代码如下：

```c++
class Solution {
public:
    unordered_map<int, int> m;
    TreeNode *buildTree(vector<int> &inorder, vector<int> &postorder) {
        if(postorder.empty()) {
            return NULL;
        }

        for(int i = 0; i < inorder.size(); i++) {
            m[inorder[i]] = i;
        }

        return build(inorder, 0, inorder.size() - 1,
            postorder, 0, postorder.size() - 1);
    }

    TreeNode* build(vector<int>& inorder, int s0, int e0,
        vector<int>& postorder, int s1, int e1) {
        if(s0 > e0 || s1 > e1) {
            return NULL;
        }

        TreeNode* root = new TreeNode(postorder[e1]);

        int mid = m[postorder[e1]];
        int num = mid - s0;

        root->left = build(inorder, s0, mid - 1, postorder, s1, s1 + num - 1);
        root->right = build(inorder, mid + 1, e0, postorder, s1 + num, e1 - 1);

        return root;
    }
};
```

这里我们需要注意，为了保证快速的在中序遍历结果里面找到根节点，我们使用了hash map。

# Construct Binary Tree from Preorder and Inorder Traversal

> Given preorder and inorder traversal of a tree, construct the binary tree.

这题跟上面那题类似，通过前序遍历和中序遍历的结果构造二叉树，我们只需要知道前序遍历的第一个值就是根节点，那么仍然可以采用上面提到的方式处理：

+ 通过前序遍历找到根节点
+ 通过根节点将中序遍历数据拆分成两部分
+ 对于各个部分重复上述操作

代码如下：

```c++
class Solution {
public:
     unordered_map<int, int> m;
    TreeNode *buildTree(vector<int> &preorder, vector<int> &inorder) {
        if(preorder.empty()) {
            return NULL;
        }

        for(int i = 0; i < inorder.size(); i++) {
            m[inorder[i]] = i;
        }

        return build(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1);
    }

    TreeNode* build(vector<int>& preorder, int s0, int e0, vector<int> &inorder, int s1, int e1) {
        if(s0 > e0 || s1 > e1) {
            return NULL;
        }

        int mid = m[preorder[s0]];

        TreeNode* root = new TreeNode(preorder[s0]);

        int num = mid - s1;

        root->left = build(preorder, s0 + 1, s0 + num, inorder, s1, mid - 1);
        root->right = build(preorder, s0 + num + 1, e0, inorder, mid + 1, e1);

        return root;
    }
};
```

可以看到，这两道题目，只要能清楚了树的几种遍历方式，以及找到如何找到根节点，并通过中序遍历拆分成两个子树，就能很容易的搞定了，唯一需要注意的是写代码的时候拆分索引位置要弄对。
