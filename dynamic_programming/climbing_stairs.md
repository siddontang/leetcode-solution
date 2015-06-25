# Climbing Stairs

> You are climbing a stair case. It takes n steps to reach to the top.

> Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

这道题目其实就是斐波那契数列问题，题目比较简单，我们很容易就能列出dp方程

`dp[n] = dp[n - 1] + dp[n - 2]`

初始条件dp[1] = 1, dp[2] = 2。

代码如下：

```c++
class Solution {
public:
    int climbStairs(int n) {
        int f1 = 2;
        int f2 = 1;
        if(n == 1) {
            return f2;
        } else if(n == 2) {
            return f1;
        }

        int fn;
        for(int i = 3; i <= n; i++) {
            fn = f1 + f2;
            f2 = f1;
            f1 = fn;
        }
        return fn;
    }
};
```
