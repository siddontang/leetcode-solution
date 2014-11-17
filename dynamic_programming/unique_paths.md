# Unique Paths

> A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

> The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

> How many possible unique paths are there?

这题是一道典型的dp问题，如果机器人要到(i, j)这个点，他可以选择先到(i - 1, j)或者，(i, j - 1)，也就是说，到(i, j)的唯一路径数等于(i - 1, j)加上(i, j - 1)的个数，所以我们很容易得出dp方程:

`dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`

dp[i][j]表示从点(0, 0)到(i, j)唯一路径数量。

代码如下：

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        int dp[m][n];
        //初始化dp，m x 1情况全为1
        for(int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }

        //初始化dp，1 x n情况全为1
        for(int j = 0; j < n; j++) {
            dp[0][j] = 1;
        }

        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }

        return dp[m - 1][n - 1];
    }
};
```

# Unique Paths II

> Now consider if some obstacles are added to the grids. How many unique paths would there be?

> An obstacle and empty space is marked as 1 and 0 respectively in the grid.

这题跟上一题唯一的区别在于多了障碍物，如果某一个点有障碍，那么机器人无法通过。

代码如下:

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int> > &obstacleGrid) {
        if(obstacleGrid.empty() || obstacleGrid[0].empty()) {
            return 0;
        }

        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        int dp[m][n];

        //下面初始dp的时候需要根据obstacleGrid的值来确定
        dp[0][0] = (obstacleGrid[0][0] == 0 ? 1 : 0);

        //我们需要注意m x 1以及1 x n的初始化
        for(int i = 1; i < m; i++) {
            dp[i][0] = ((dp[i - 1][0] == 1 && obstacleGrid[i][0] == 0) ? 1 : 0);
        }

        for(int j = 1; j < n; j++) {
            dp[0][j] = ((dp[0][j - 1] == 1 && obstacleGrid[0][j] == 0) ? 1 : 0);
        }


        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                if(obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                } else {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }

        return dp[m - 1][n - 1];
    }
};
```

# Minimum Path Sum

> Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

> Note: You can only move either down or right at any point in time.

这题跟前面两题差不多，所以放到这里说明了。我们使用dp[i][j]表明从(0, 0)到(i, j)最小的路径和，那么dp方程为:

`dp[i][j] = min(dp[i][j-1], dp[i - 1][j]) + grid[i][j]`

代码如下：

```c++
class Solution {
public:
    int minPathSum(vector<vector<int> > &grid) {
        if(grid.empty() || grid[0].empty()) {
            return 0;
        }

        int row = grid.size();
        int col = grid[0].size();

        int dp[row][col];

        dp[0][0] = grid[0][0];
        for(int i = 1; i < row; i++) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }

        for(int j = 1; j < col; j++) {
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }

        for(int i = 1; i < row; i++) {
            for(int j = 1; j < col; j++) {
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }

        return dp[row - 1][col - 1];
    }
};
```
