# Gas Station

> There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

> You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

> Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

> Note:
> The solution is guaranteed to be unique.

这题的意思就是求出从哪一个油站开始，能走完整个里程，并且这个结果是唯一的。

首先我们可以得到所有油站的油量totalGas，以及总里程需要消耗的油量totalCost，如果totalCost大于totalGas，那么铁定不能够走完整个里程。

如果totalGas大于totalCost了，那么就能走完整个里程了，假设现在我们到达了第i个油站，这时候还剩余的油量为sum，如果 sum + gas[i] - cost[i]小于0，我们无法走到下一个油站，所以起点一定不在第i个以及之前的油站里面（都铁定走不到第i + 1号油站），起点只能在i + 1后者后面。

代码如下：

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
        int sum = 0;
        int total = 0;
        int k = 0;
        for(int i = 0; i < (int)gas.size(); i++) {
            sum += gas[i] - cost[i];
            //小于0就只可能在i + 1或者之后了
            if(sum < 0) {
                k = i + 1;
                sum = 0;
            }
            total += gas[i] - cost[i];
        }

        if(total < 0) {
            return -1;
        } else {
            return k;
        }
    }
};
```
