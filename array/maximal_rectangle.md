# Maximal Rectangle

> Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing all ones and return its area.

这题是一道难度很大的题目，至少我刚开始的时候完全不知道怎么做，也是google了才知道的。

这题要求在一个矩阵里面求出全部包含1的最大矩形面积，譬如这个：

```
    0 0 0 0
    1 1 1 1
    1 1 1 0
    0 1 0 0
```

我们可以知道，最大的矩形面积为6。也就是下图中虚线包围的区域。那么我们如何得到这个区域呢？

```
    0  0  0  0
   |--------|
   |1  1  1 |1
   |1  1  1 |0
   |--------|
    0  1  0  0
```

对于上面哪一题，我们先去掉最下面的一行，然后就可以发现，它可以转化成一个直方图，数据为[2, 2, 2, 0]，我们认为1就是高度，如果碰到0，譬如上面最右列，则高度为0，而计算这个直方图最大矩形面积就很容易了，我们已经在[Largest Rectangle in Histogram](array/largest_rectangle_in_histogram.md)处理了。

所以我们可以首先得到每一行的直方图，分别求出改直方图的最大区域，最后就能得到结果了。

代码如下：

```c++
class Solution {
public:
    int maximalRectangle(vector<vector<char> > &matrix) {
        if(matrix.empty() || matrix[0].empty()) {
            return 0;
        }

        int m = matrix.size();
        int n = matrix[0].size();

        vector<vector<int> > height(m, vector<int>(n, 0));
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(matrix[i][j] == '0') {
                    height[i][j] = 0;
                } else {
                    height[i][j] = (i == 0) ? 1 : height[i - 1][j] + 1;
                }
            }
        }

        int maxArea = 0;
        for(int i = 0; i < m; i++) {
            maxArea = max(maxArea, largestRectangleArea(height[i]));
        }
        return maxArea;
    }

    int largestRectangleArea(vector<int> &height) {
        vector<int> s;
        height.push_back(0);

        int sum = 0;
        int i = 0;
        while(i < height.size()) {
            if(s.empty() || height[i] > height[s.back()]) {
                s.push_back(i);
                i++;
            } else {
                int t = s.back();
                s.pop_back();
                sum = max(sum, height[t] * (s.empty() ? i : i - s.back() - 1));
            }
        }

        return sum;
    }
};
```


