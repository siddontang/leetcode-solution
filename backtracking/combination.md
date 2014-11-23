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

# Combination Sum

> Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

> The same repeated number may be chosen from C unlimited number of times.

 Note:
 1. All numbers (including target) will be positive integers.
 2. Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
 3. The solution set must not contain duplicate combinations.

题目翻译:
给一个数组C和一个目标值T, 找出所有的满足条件的组合：使得组合里面的数字之和等于T,并且一些数字可以从C中午先选择。

注意:
1. 所有给定的数字均为正整数.(这意味着我们加corner case invalid check的时候要加一条，如果给定T不是正整数，我们就没必要在往下进行了)

2. 所有的答案组中要满足升序排列.

3. 最后的答案数组不能包含重复答案.

题目分析:
这道题的大体思路和combination是相同的，不同的地方在于一个数字可以使用多次，这也造成了我们进行实现function的时候要注意的问题，也就是说，传入递归的参数不同于combination.

时间复杂度:
没什么好说的，和combination的时间复杂度是相同的.O(n!)

代码如下:

```c++
class Solution {
public:
    vector<vector<int> > combinationSum(vector<int> &candidates, int target) {
        vector<vector<int>> ret;
        //corner case invalid check
        if(candidates.size() == 0 || target < 0)
            return ret;
        vector<int> curr;
        sort(candidates.begin(),candidates.end()); //because the requirments need the elements should be in non-descending order
        BackTracking(ret,curr,candidates,target,0);
        return ret;
    }

    /* we use reference at here because the function return type is 0, make the code understand easily */
    void BackTracking(vector<vector<int>>& ret, vector<int> curr, vector<int> candidates, int target, int level)
    {
        if(target == 0)
        {
            ret.push_back(curr);
            return;
        }
        else if(target < 0) //save time
            return;

        for(int i = level; i < candidates.size(); ++i)
        {
            target -= candidates[i];
            curr.push_back(candidates[i]);
            BackTracking(ret,curr,candidates,target,i); //unlike combination, we do not use i+1 because we can use the same number multiple times.
            curr.pop_back();
            target += candidates[i];
        }
    }

};
```

# Combination Sum II

> Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

> Each number in C may only be used once in the combination.

Note:

1. All numbers (including target) will be positive integers.

2. Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).

3. The solution set must not contain duplicate combinations.


题目翻译:
给定一个数组C和一个特定值T,要求找出这里面满足以下条件的所有答案：数组中数字的值加起来等于特定和的答案.

数组中每个数字只能用一次.（同three sum和four sum的解法）

注意条件:
1. 给定数组的所有值必须是正整数.（意味着我们加corner case invalid check的时候要检查T）
2. 答案数组中的值必须为升序排列.(我们要对数组进行排序)
3. 最终答案不能包含重复数组.

代码如下:
```c++
class Solution {
public:
    vector<vector<int> > combinationSum2(vector<int> &num, int target) {
        vector<vector<int>> ret;
        if(num.size() == 0 || target < 0) //invalid corner case check
            return ret;
        vector<int> curr;
        sort(num.begin(), num.end());
        BackTracking(ret,curr,num,target,0);
        return ret;
    }

    void BackTracking(vector<vector<int>>& ret, vector<int> curr, vector<int> num, int target, int level)
    {
        if(target == 0)
        {
            ret.push_back(curr);
            return;
        }
        else if(target < 0)
            return;
        for(int i = level; i < num.size(); ++i)
        {
            target -= num[i];
            curr.push_back(num[i]);
            BackTracking(ret,curr,num,target,i+1);
            curr.pop_back();
            target += num[i];
            while(i < num.size()-1 && num[i] == num[i+1]) //we add this while loop is to skip the duplication result
                ++i;
        }
    }

};
```

# Letter Combinations of a Phone Number

> Given a digit string, return all possible letter combinations that the number could represent.  A mapping of digit to letters (just like on the telephone buttons) is given as below:
![](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
题目翻译:
给定一个字符串数字，返回这个字符串数字在电话表上可能的combination，一个map的电话键盘在上图已经给出.

题目分析:
这道题目的给出，具体的解题思路是和combination是相同的，不同的地方是我们要先建一个dictionary,以方便查找.之后用combination的相同方法，对于每一个数字，在dictionary中查找它所对应的说有的数字.

解题思路:
我是用字符串数组来构建这个dictionary的，用于下标代表数字，例如，下标为2，我的字典就会有这种对应的关系:dic[2] = "abc".只要把给定数字字符串的每一个数字转换为int类型，就可以根据字典查找出这个数字所对应的所有字母.当然，再构建字典的时候，我们需要注意dic[0] = "", dic[1] = "".这两个特殊的case，因为电话键盘并没有这两个数字相对应的字符串.

时间复杂度:
O(3^n)


代码如下:

```c++

class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ret;
        /* for this question, we need to create a look-up dictionary */
        vector<string> dic;
        string tmp;
        dic.push_back(" ");
        dic.push_back(" ");
        dic.push_back("abc");
        dic.push_back("def");
        dic.push_back("ghi");
        dic.push_back("jkl");
        dic.push_back("mno");
        dic.push_back("pqrs");
        dic.push_back("tuv");
        dic.push_back("wxyz");
        combinations(ret,tmp,digits,dic,0);
        return ret;
    }

    void combinations(vector<string>& ret, string tmp, string digits, vector<string> dic, int level)
    {
        if(level == digits.size())
        {
            ret.push_back(tmp);
            return;
        }

        int index = digits[level]-'0';
        for(int i = 0; i < dic[index].size(); ++i)
        {
            tmp.push_back(dic[index][i]);
            combinations(ret,tmp,digits,dic,level+1);
            tmp.pop_back();
        }
    }


};
```


