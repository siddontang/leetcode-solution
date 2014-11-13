# Remove Duplicates from Sorted Array

> Given a sorted array, remove the duplicates in place such that > each element appear only once and return the new length.

> Do not allocate extra space for another array, you must do this in place with constant memory.

> For example,
> Given input array A = [1,1,2],

> Your function should return length = 2, and A is now [1,2].

这道题目与前一题Remove Element比较类似。首先我们需要知道，对于一个排好序的数组来说，`A[N + 1] >= A[N]`，我们仍然使用两个游标i和j来处理，假设现在i = j + 1，如果A[i] == A[j]，那么我们递增i，直到A[i] != A[j]，这时候我们再A[j + 1] = A[i]，递增i和j，重复直到遍历结束。

代码如下：

```c++
class Solution {
public:
    int removeDuplicates(int A[], int n) {
        if(n == 0) {
            return 0;
        }

        int j = 0;
        for(int i = 1; i < n; i++) {
            if(A[j] != A[i]) {
                A[++j] = A[i];
            }
        }
        return j + 1;
    }
};
```

譬如一个数组为1，1，2，3，首先i = 1，j = 0，这时候A[i] = A[j]，所以递增i，碰到2，不等于1，所以A[j + 1] = A[i]，也就是A[1] = A[2]，递增i和j为3和1，这时候A[3] != A[1]，所以A[j + 1] = A[i]，也就是A[2] = A[3]，再次递增，遍历结束。这时候新的数组长度就为2 + 1，也就是3。
