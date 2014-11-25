# Unique Binary Search Trees

> Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

> For example,
> Given n = 3, there are a total of 5 unique BST's.

>```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
>

这道题目要求给定一个数n，有多少种二叉树排列方式，用来存储1到n。

刚开始拿到这题的时候，完全不知道如何下手，但考虑到二叉树的性质，对于任意以i为根节点的二叉树，它的左子树的值一定小于i，也就是[0, i - 1]区间，而右子树的值一定大于i，也就是[i + 1, n]。假设左子树有m种排列方式，而右子树有n种，则对于i为根节点的二叉树总的排列方式就是m x n。

我们使用dp[i]表示i个节点下面二叉树的排列个数，那么dp方程为:

`dp[i] = sum(dp[k] * dp[i - k -1]) 0 <= k < i`

代码如下：

```c++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1, 0);

        //dp初始化
        dp[0] = 1;
        dp[1] = 1;

        for(int i = 2; i <= n; i++) {
            for(int j = 0; j < i; j++) {
                //如果左子树的个数为j，那么右子树为i - j - 1
                dp[i] += dp[j] * dp[i - j - 1];
            }
        }

        return dp[n];
    }
};
```

# Unique Binary Search Trees II

> Given n, generate all structurally unique BST's (binary search trees) that store values 1...n.

> For example,
Given n = 3, your program should return all 5 unique BST's shown below.

>```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
>

这题跟前面一题不同，需要得到所有排列的解。

根据前面我们知道，对于在n里面的任意i，它的排列数为左子树[0, i - 1]的排列数 x 右子树[i + 1, n]的排列数，所以我们只需要得到i的左子树和右子树的所有排列，就能得到i的所有排列了。而这个使用递归就能搞定，代码如下：

```c++
class Solution {
public:
    vector<TreeNode *> generateTrees(int n) {
        return generate(1, n);
    }

    vector<TreeNode*> generate(int start, int stop){
        vector<TreeNode*> vs;
        if(start > stop) {
            //没有子树了，返回null
            vs.push_back(NULL);
            return vs;
        }

        for(int i = start; i <= stop; i++) {
            auto l = generate(start, i - 1);
            auto r = generate(i + 1, stop);

            //获取左子树和右子树所有排列之后，放到root为i的节点的下面
            for(int j = 0; j < l.size(); j++) {
                for(int k = 0; k < r.size(); k++) {
                    TreeNode* n = new TreeNode(i);
                    n->left = l[j];
                    n->right = r[k];
                    vs.push_back(n);
                }
            }
        }

        return vs;
    }
};
```
