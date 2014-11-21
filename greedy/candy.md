# Candy

> There are N children standing in a line. Each child is assigned a rating value.

> You are giving candies to these children subjected to the following requirements:

> Each child must have at least one candy.
> Children with a higher rating get more candies than their neighbors.
What is the minimum candies you must give?

好了，终于到了小盆友，排队领糖果的时候了，我们可是坏叔叔。

这题要求每个小孩至少要领到一颗糖果，但是高级别的小孩要比它旁边的孩子得到的糖果多（小孩的世界也有不平等了），问最少需要发多少糖果？

首先我们会给每个小朋友一颗糖果，然后从左到右，假设第i个小孩的等级比第i - 1个小孩高，那么第i的小孩的糖果数量就是第i - 1个小孩糖果数量在加一。再我们从右到左，如果第i个小孩的等级大于第i + 1个小孩的，同时第i个小孩此时的糖果数量小于第i + 1的小孩，那么第i个小孩的糖果数量就是第i + 1个小孩的糖果数量加一。

代码如下：

```c++
class Solution {
public:
    int candy(vector<int> &ratings) {
        vector<int> candys;
        //首先每人发一颗糖
        candys.resize(ratings.size(), 1);
        //这个循环保证了右边高级别的孩子一定比左边的孩子糖果数量多
        for(int i = 1; i < (int)ratings.size(); i++) {
            if(ratings[i] > ratings[i - 1]) {
                candys[i] = candys[i - 1] + 1;
            }
        }

        //这个循环保证了左边高级别的孩子一定比右边的孩子糖果数量多
        for(int i = (int)ratings.size() - 2; i >= 0; i--) {
            if(ratings[i] > ratings[i + 1] && candys[i] <= candys[i + 1]) {
                candys[i] = candys[i + 1] + 1;
            }
        }

        int n = 0;
        for(int i = 0; i < (int)candys.size(); i++) {
            n += candys[i];
        }
        return n;
    }
};
```
