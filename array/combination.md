# Combination

在这个section里面，我们主要来过一下像leetcode里面类似combination这一系列的题，这类题应该归结为DFS+Backtracking。掌握了大体思想，注意一下边角处理就好，比如剪枝。

先来讨论一下第一题Combination.

#Combination

> Given two integers n and k, return all possible combinations of k numbers out of 1,2,...,n.

题目翻译:
给定两个整型数组n和k，要求返由k个数组成的combination，其实应该叫做组合. 这个combination应该是高中里面的组合。这k个数是在n中任选k个数，由题意可得，这里的k应该小于或等于n(这个条件不要忘了做validation check哦).

题目分析:
我觉得应该还有不少读者困惑什么是combination，这里我们先给一个例子比如n=3， k=2的条件下， 所有可能的combination如下：
[1,2], [1,3], [2,3]. 注意：[2,3]和[3,2]是相同的，我们只要求有其中一个就可以了.
所以解题的时候，我们要避免相同的组合出现.

解题思路:
看到这道题，首先第一想法应该是用递归来求解.如果要是用循环来求解，这个时间复杂度应该是比较恐怖了.并且，这个递归是一层一层往深处去走的，打个比方，我们一个循环，首先求得以1开始的看个数的combination，之后再求以2开始的，以此类推，所以开始是对n个数做DFS, n-1个数做DFS...所以应该是对n*(n-1)*...*1做DFS. 在程序中，我们可以加一些剪枝条件来减少程序时间.


时间复杂度:
在题目分析中，我们提到了对于对n,n-1,...,1做DFS，所以时间复杂度是O(n!)

代码如下:

```c++

class Solution {
public:
    vector<vector<int> > combine(int n, int k) {
        vector<vector<int>> ret;
        if(n <= 0) //corner case invalid check
            return ret;

        vector<int> curr;
        DFS(ret,curr, n, k, 1); //we pass ret as reference at here
        return ret;
    }

    void DFS(vector<vector<int>>& ret, vector<int> curr, int n, int k, int level)
    {
        if(curr.size() == k)
        {
            ret.push_back(curr);
            return;
        }
        if(curr.size() > k)  // consider this check to save run time
            return;

        for(int i = level; i <= n; ++i)
        {
            curr.push_back(i);
            DFS(ret,curr,n,k,i+1);
            curr.pop_back();
        }
    }


};
```


