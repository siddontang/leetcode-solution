# Maximum Subarray

> Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

> For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
the contiguous subarray [4,−1,2,1] has the largest sum = 6.

这题是一道经典的dp问题，我们可以很容易的得到其dp方程，假设dp[i]是数组a [0, i]区间最大的值，那么

`dp[i + 1] = max(dp[i], dp[i] + a[i + 1])`

代码如下:

```c++
class Solution {
public:
    int maxSubArray(int A[], int n) {
        int sum = 0;
        int m = INT_MIN;

        for(int i = 0; i < n; i++) {
            sum += A[i];
            m = max(m, sum);
            //如果sum小于0了，将sum重置为0，从i + 1再次开始计算
            if(sum < 0) {
                sum = 0;
            }
        }

        return m;
    }
};
```

虽然这道题目用dp解起来很简单，但是题目说了，问我们能不能采用divide and conquer的方法解答，也就是二分法。

假设数组A[left, right]存在最大区间，mid = (left + right) / 2，那么无非就是三中情况：

1. 最大值在A[left, mid - 1]里面
2. 最大值在A[mid + 1, right]里面
3. 最大值跨过了mid，也就是我们需要计算[left, mid - 1]区间的最大值，以及[mid + 1, right]的最大值，然后加上mid，三者之和就是总的最大值

我们可以看到，对于1和2，我们通过递归可以很方便的求解，然后在同第3的结果比较，就是得到的最大值。

代码如下:

```c++
class Solution {
public:
    int maxSubArray(int A[], int n) {
        return divide(A, 0, n - 1, INT_MIN);
    }

    int divide(int A[], int left, int right, int tmax) {
        if(left > right) {
            return INT_MIN;
        }

        int mid = left + (right - left) / 2;
        //得到子区间[left, mid - 1]最大值
        int lmax = divide(A, left, mid - 1, tmax);
        //得到子区间[mid + 1, right]最大值
        int rmax = divide(A, mid + 1, right, tmax);

        tmax = max(tmax, lmax);
        tmax = max(tmax, rmax);

        int sum = 0;
        int mlmax = 0;
        //得到[left, mid - 1]最大值
        for(int i = mid - 1; i >= left; i--) {
            sum += A[i];
            mlmax = max(mlmax, sum);
        }

        sum = 0;
        int mrmax = 0;
        //得到[mid + 1, right]最大值
        for(int i = mid + 1; i <= right; i++) {
            sum += A[i];
            mrmax = max(mrmax, sum);
        }

        tmax = max(tmax, A[mid] + mlmax + mrmax);
        return tmax;
    }
};
```

# Maxmimum Product Subarray

> Find the contiguous subarray within an array (containing at least one number) which has the largest product.

> For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.

这题是求数组中子区间的最大乘积，对于乘法，我们需要注意，负数乘以负数，会变成正数，所以解这题的时候我们需要维护两个变量，当前的最大值，以及最小值，最小值可能为负数，但没准下一步乘以一个负数，当前的最大值就变成最小值，而最小值则变成最大值了。

我们的动态方程可能这样：

```
maxDP[i + 1] = max(maxDP[i] * A[i + 1], A[i + 1], minDP[i] * A[i + 1])
minDP[i + 1] = min(minDP[i] * A[i + 1], A[i + 1], maxDP[i] * A[i + 1]
dp[i + 1] = max(dp[i], maxDP[i + 1])
```

这里，我们还需要注意元素为0的情况，如果A[i]为0，那么maxDP和minDP都为0，我们需要从A[i + 1]重新开始。

代码如下:

```c++
class Solution {
public:
    int maxProduct(int A[], int n) {
        if(n == 0){
            return 0;
        } else if(n == 1) {
            return A[0];
        }

        int p = A[0];
        int maxP = A[0];
        int minP = A[0];
        for(int i = 1; i < n; i++) {
            int t = maxP;
            maxP = max(max(maxP * A[i], A[i]), minP * A[i]);
            minP = min(min(t * A[i], A[i]), minP * A[i]);
            p = max(maxP, p);
        }

        return p;
    }
};
```
