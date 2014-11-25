# Permutation

Permutation这个分支是在backtracking下的一个子分支，其具体的解题方法和Combination几乎是同出一辙，一个思路，对于给定数组用DFS方法一层一层遍历，在这个section当中，我们将对于leetcode上出现的permutation问题进行逐个分析与解答.

# Permutations

> given a collection of numbers, return all posibile permutations.

> For example, [1,2,3] have the following permutations:
[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], and [3,2,1].

题目翻译:
给定一个整形数组，要求求出这个数组的所有变形体，具体例子看上文中给出的例子就可以.

题目分析:
这道题很直接，几乎算是没有坑，相信大家都可以理解题目的要求. Permutation的解题方法和Combination几乎是相同的，唯一需要注意的是，Permutation需要加一个bool类型的数组来进行记录哪个元素访问了，哪个没有，这样才不会导致重复出现，并且不同于Combination的一点是，Permutation不需要排序.

解题思路:
遇到这种问题，很显然，第一个想法我们首先回去想到DFS,递归求解，对于数组中的每一个元素，找到以他为首节点的Permutations,这就要求在递归中，每次都要从数组的第一个元素开始遍历，这样，，就引入了另外一个问题，我们会对于同一元素访问多次，这就不是我们想要的答案了，所以我们引入了一个bool类型的数组，用来记录哪个元素被遍历了(通过下标找出对应).在对于每一个Permutation进行求解中，如果访问了这个元素,我们将它对应下表的bool数组中的值置为true,访问结束后，我们再置为false.

时间复杂度分析:
这道题同Combination,所以对于这道题的解答，时间复杂度同样是O(n!)

代码如下:
```c++

class Solution {
public:
    vector<vector<int> > permute(vector<int> &num) {
        vector<vector<int>> permutations;
        if(num.size() == 0) //invalid corner case check
            return permutations;
        vector<int> curr;
        vector<bool> isVisited(num.size(), false); //using this bool type array to check whether or not the elments has been visited
        backTracking(permutations,curr,isVisited,num);
        return permutations;
    }

    void backTracking(vector<vector<int>>& ret, vector<int> curr, vector<bool> isVisited, vector<int> num)
    {
        if(curr.size() == num.size())
        {
            ret.push_back(curr);
            return;
        }

        for(int i = 0; i < num.size(); ++i)
        {
            if(isVisited[i] == false)
            {
                isVisited[i] = true;
                curr.push_back(num[i]);
                backTracking(ret,curr,isVisited,num);
                isVisited[i] = false;
                curr.pop_back();
            }
        }
    }


};
```

# Permutations II

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.

> For example,
[1,1,2] have the following unique permutations:
[1,1,2], [1,2,1], and [2,1,1].

题目翻译:
给定一个整形数组，这个数组中可能会包含重复的数字，要求我们返回的是这个数组不同的Permutations,也就是说每一种可能的permutation在最后的答案中只能出现一次.上文的例子能清晰的告诉读者不同的地方.

题目分析:
对于这道题。也是要求permutation,大体上的解题思路和Permutations是相同的，但是不同点在哪里呢？
不同点为：
1. 这个给定的数组中可能会含有相同的数字.
2. 最后答案不接受重复相同的答案组.

对于这两点要求，Permutations的解法是无法解决的，所以我们就要考虑怎样满足以上两个要求. 我们可以对整个input数组进行排序，在求解答案的时候，只要一个元素的permutation求出来了，在这个元素后面和这个元素相同的元素，我们完全都可以pass掉，其实这个方法在sum和combination里面已经是屡试不爽了.

解题思路:
除了加上对于重复答案的处理外，剩下思路同Permutation一模一样。

时间复杂度:
O(n!)

代码如下:
```c++

class Solution {
public:
    vector<vector<int> > permuteUnique(vector<int> &num) {
        vector<vector<int>> permutations;
        if(num.size() == 0)
            return permutations;
        vector<int> curr;
        vector<bool> isVisited(num.size(), false);
        /* we need to sort the input array here because of this array
           contains the duplication value, then we need to skip the duplication
           value for the final result */
        sort(num.begin(),num.end());
        DFS(permutations,curr,num,isVisited);
        return permutations;
    }

    void DFS(vector<vector<int>>& ret, vector<int> curr, vector<int> num, vector<bool> isVisited)
    {
        if(curr.size() == num.size())
        {
            ret.push_back(curr);
            return;
        }

        for(int i = 0; i < num.size(); ++i)
        {
            if(isVisited[i] == false)
            {
                isVisited[i] = true;
                curr.push_back(num[i]);
                DFS(ret,curr,num,isVisited);
                isVisited[i] = false;
                curr.pop_back();
                while(i < num.size()-1 && num[i] == num[i+1]) //we use this while loop to skip the duplication value in the input array.
                    ++i;
            }
        }
    }
};

```

