# Subsets

> Given a set of distinct integers, S, return all possible subsets.

> Note:
> Elements in a subset must be in non-descending order.
The solution set must not contain duplicate subsets.
For example,
If S = [1,2,3], a solution is:

>
```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
>

这题其实就是列出集合里面的所有子集合，同时要求子集合元素需要升序排列。

首先我们需要对集合排序，对于一个n元素的集合，首先我们取第一个元素，加入子集合中，后面的n - 1个元素可以认为是第一个元素的子节点，我们依次遍历，譬如遍历到第二个元素的时候，后续的n - 2个元素又是第二个元素的子节点，再依次遍历处理，直到最后一个元素，然后回溯，继续处理。处理完第一个元素之后，我们按照同样的方式处理第二个元素。

譬如上面的[1, 2, 3]，首先取出1，加入子集合，后面的2和3就是1的子节点，先取出2，把[1, 2]加入子集合，后面的3就是2的子节点，取出3，把[1, 2, 3]加入子集合。然后回溯，取出3，将[1, 3]加入子集合。

1处理完成之后，我们可以同样方式处理2，以及3。

代码如下：

```c++
class Solution {
public:
    vector<vector<int> > res;
    vector<vector<int> > subsets(vector<int> &S) {
        if(S.empty()) {
            return res;
        }

        sort(S.begin(), S.end());

        //别忘了空集合
        res.push_back(vector<int>());

        vector<int> v;

        generate(0, v, S);

        return res;
    }

    void generate(int start, vector<int>& v, vector<int> &S) {
        if(start == S.size()) {
            return;
        }

        for(int i = start; i < S.size(); i++) {
            v.push_back(S[i]);

            res.push_back(v);

            generate(i + 1, v, S);

            v.pop_back();
        }
    }
};
```

# Subsets II

> Given a collection of integers that might contain duplicates, S, return all possible subsets.

> Note:
> Elements in a subset must be in non-descending order.
The solution set must not contain duplicate subsets.
For example,
If S = [1,2,2], a solution is:

>
```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```
>

这题跟上题唯一的区别在于有重复元素，但是我们得到的子集合又不能有相同的，其实做法很简单，仍然按照上面的处理，只是遍历子节点的时候如果发现有相等的，只遍历一个，后续跳过。代码如下：

```c++
class Solution {
public:
    vector<vector<int> > res;

    vector<vector<int> > subsetsWithDup(vector<int> &S) {
        if(S.empty()) {
            return res;
        }

        sort(S.begin(), S.end());

        res.push_back(vector<int>());

        vector<int> v;

        generate(0, v, S);

        return res;
    }

    void generate(int start, vector<int>& v, vector<int> &S) {
        if(start == S.size()) {
            return;
        }

        for(int i = start; i < S.size(); i++) {
            v.push_back(S[i]);

            res.push_back(v);

            generate(i + 1, v, S);

            v.pop_back();

            //这里跳过相同的
            while(i < S.size() - 1 && S[i] == S[i + 1]) {
                i++;
            }
        }
    }
};
```
