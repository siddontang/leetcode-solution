# Largest Rectangle in Histogram

> Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

> ![histogram](http://www.leetcode.com/wp-content/uploads/2012/04/histogram.png)

> Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].

> ![histogram_area](http://www.leetcode.com/wp-content/uploads/2012/04/histogram_area.png)

> The largest rectangle is shown in the shaded area, which has area = 10 unit.

> For example,
> Given height = [2,1,5,6,2,3],
> return 10.

这道题目算是比较难得一道题目了，首先最简单的做法就是对于任意一个bar，向左向右遍历，直到高度小于该bar，这时候计算该区域的矩形区域面积。对于每一个bar，我们都做如上处理，最后就可以得到最大值了。当然这种做法是O(n2)，铁定过不了大数据集合测试的。

从上面我们直到，对于任意一个bar n，我们得到的包含该bar n的矩形区域里面bar n是最小的。我们使用ln和rn来表示bar n向左以及向右第一个小于bar n的bar的索引位置。

譬如题目中的bar 2的高度为5，它的ln为1，rn为4。包含bar 2的矩形区域面积为(4 - 1 - 1) * 5 = 10。

我们可以从左到右遍历所有bar，并将其push到一个stack中，如果当前bar的高度小于栈顶bar，我们pop出栈顶的bar，同时以该bar计算矩形面积。那么我们如何知道该bar的ln和rn呢？rn铁定就是当前遍历到的bar的索引，而ln则是当前的栈顶bar的索引，因为此时栈顶bar的高度一定小于pop出来的bar的高度。

为了更好的处理最后一个bar的情况，我们在实际中会插入一个高度为0的bar，这样就能pop出最后一个bar并计算了。

代码如下：

```c++
class Solution {
public:
    int largestRectangleArea(vector<int> &height) {
        vector<int> s;
        //插入高度为0的bar
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
                //这里还需要考虑stack为空的情况
                sum = max(sum, height[t] * (s.empty() ? i : i - s.back() - 1));
            }
        }

        return sum;
    }
};

```
