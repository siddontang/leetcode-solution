# Jump Game

> Given an array of non-negative integers, you are initially positioned at the first index of the array.

> Each element in the array represents your maximum jump length at that position.

> Determine if you are able to reach the last index.

> For example:
> A = [2,3,1,1,4], return true.

> A = [3,2,1,0,4], return false.

这题比较简单，给你一个数组，里面每个元素表示你可以向后跳跃的步数，我们需要知道能不能移动到最后一个元素位置。

采用贪心法即可，譬如上面的[2, 3, 1, 1, 4]，因为初始第一个位置为2，我们先跳1步，剩下1步了，到第二个元素位置，也就是3这个地方，因为3比1大，所以我们可以向后面跳跃3步，直接就到4了。

根据上面的规则，每次跳跃1步，我们可跳跃步数减1，如果新的位置步数大于剩余步数，使用新的步数继续移动，如果可跳跃次数小于0并且还没到最后一个元素，那么失败。

代码如下:

```c++
class Solution {
public:
    bool canJump(int A[], int n) {
        if(n == 0) {
            return true;
        }

        int v = A[0];

        for(int i = 1; i < n; i++) {
            v--;
            if(v < 0) {
                return false;
            }

            if(v < A[i]) {
                v = A[i];
            }
        }
        return true;
    }
};
```

# Jump Game II

> Given an array of non-negative integers, you are initially positioned at the first index of the array.

> Each element in the array represents your maximum jump length at that position.

> Your goal is to reach the last index in the minimum number of jumps.

> For example:
> Given array A = [2,3,1,1,4]

> The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)

这题不同于上一题，只要求我们得到最少的跳跃次数，所以铁定能走到终点的，我们仍然使用贪心法，

我们维护两个变量，当前能达到的最远点p以及下一次能达到的最远点q，在p的范围内迭代计算q，然后更新步数，并将最大的q设置为p。重复这个过程知道p达到终点。

代码如下：

```c++
class Solution {
public:
    int jump(int A[], int n) {
        int step = 0;
        int cur = 0;
        int next = 0;

        int i = 0;
        while(i < n){
            if(cur >= n - 1) {
                break;
            }

            while(i <= cur) {
                //更新最远达到点
                next = max(next, A[i] + i);
                i++;
            }
            step++;
            cur = next;
        }

        return step;
    }
};
```

