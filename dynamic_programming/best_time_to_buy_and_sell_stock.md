# Best Time to Buy and Sell Stock

> Say you have an array for which the ith element is the price of a given stock on day i.

> If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

这是卖股票的第一个题目，根据题意我们知道只能进行一次交易，但需要获得最大的利润，所以我们需要在最低价买入，最高价卖出，当然买入一定要在卖出之前。

对于这一题，还是比较简单的，我们只需要遍历一次数组，通过一个变量记录当前最低价格，同时算出此次交易利润，并与当前最大值比较就可以了。

代码如下：

```c++
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        if(prices.size() <= 1) {
            return 0;
        }

        int minP = prices[0];

        int profit = prices[1] - prices[0];

        for(int i = 2; i < prices.size(); i++) {
            minP = min(prices[i - 1], minP);
            profit = max(profit, prices[i] - minP);
        }

        if(profit < 0) {
            return 0;
        }

        return profit;
    }
};
```

# Best Time to Buy and Sell Stock II

> Say you have an array for which the ith element is the price of a given stock on day i.

> Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

这题相对于上一题来说更加容易（这不知道为啥是II），因为不限制交易次数，我们在第i天买入，如果发现i + 1天比i高，那么就可以累加到利润里面。

代码如下：

```c++
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int len = (int)prices.size();
        if(len <= 1) {
            return 0;
        }

        int sum = 0;
        for(int i = 1; i < len; i++) {
            if(prices[i] - prices[i - 1] > 0) {
                sum += prices[i] - prices[i - 1];
            }
        }

        return sum;
    }
};
```

# Best Time to Buy and Sell Stock III

> Say you have an array for which the ith element is the price of a given stock on day i.

> Design an algorithm to find the maximum profit. You may complete at most two transactions.

> Note:
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

这题是三道题目中最难的一题，只允许两次股票交易，如果只允许一次，那么题目就退化到第一题了，根据第一题的算法，我们可以得到[0,1,...,i]区间的最大利润，同时在从后往前扫描得到[i,i+1,...,n-1]的最大利润，两个相加就可以得到该题的解了。

代码如下：

```c++
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int len = (int)prices.size();
        if(len <= 1) {
            return 0;
        }

        vector<int> profits;
        profits.resize(len);

        //首先我们正向遍历得到每天一次交易的最大收益
        //并保存到profits里面
        int minP = prices[0];
        int sum = numeric_limits<int>::min();
        for(int i = 1; i < len; i++) {
            minP = min(minP, prices[i - 1]);
            profits[i] = max(sum, prices[i] - minP);

            sum = profits[i];
        }

        int maxP = prices[len - 1];
        int sum2 = numeric_limits<int>::min();

        //逆向遍历
        for(int i = len - 2; i >= 0; i--) {
            maxP = max(maxP, prices[i + 1]);
            sum2 = max(sum2, maxP - prices[i]);

            if(sum2 > 0) {
                //这里我们直接将其加入profits里面，
                //不需要额外保存
                profits[i] = profits[i] + sum2;
                sum = max(sum, profits[i]);
            }
        }

        return sum > 0 ? sum : 0;
    }
};
```

卖股票的1和3已经涉及到了动态规划，但笔者这方面还未有太多知识积累，所以不能很好的列举出动态规划方程，这是笔者后续需要努力提升的，后续补上。
