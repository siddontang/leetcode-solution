# Search a 2D Matrix

> Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

> Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
For example,

> Consider the following matrix:

```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
题目翻译:
给定一个矩阵和一个特定值，要求写出一个高效的算法在这个矩阵中快速的找出是否这个给定的值存在.
但是这个矩阵有以下特征.

1. 对于每一行，数值是从左到右从小到大排列的.
2. 对于每一列，数值是从上到下从小到大排列的.


题目解析:
对于这个给定的矩阵，我们如果用brute force解法，用两个嵌套循环，O(n2)便可以得到答案.但是我们需要注意的是这道题已经给定了这个矩阵的两个特性，这两个特性对于提高我们算法的时间复杂度有很大帮助，首先我们给出一个O(n)的解法，也就是说我们可以固定住右上角的元素，根据递增或者递减的规律，我们可以判断这个给定的数值是否存在于这个矩阵当中.

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int> > &matrix, int target) {
        /* we set the corner case as below:
           1, if the row number of input matrix is 0, we set it false
           2, if the colomun number of input matrix is 0, we set it false*/
        if(matrix.size() == 0)
            return false;
        if(matrix[0].size() == 0)
            return false;
        int rowNumber = 0;
        int colNumber = matrix[0].size()-1;
        while(rowNumber < matrix.size() && colNumber >= 0)
        {
            if(target < matrix[rowNumber][colNumber])
                --colNumber;
            else if(target > matrix[rowNumber][colNumber])
                ++rowNumber;
            else
                return true;
        }
        return false;
    }
};

```
