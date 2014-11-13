# Remove Duplicates from Sorted Array

> Given a sorted array, remove the duplicates in place such that > each element appear only once and return the new length.

> Do not allocate extra space for another array, you must do this in place with constant memory.

> For example,
> Given input array A = [1,1,2],

> Your function should return length = 2, and A is now [1,2].

这道题目与前一题Remove Element比较类似。但是在一个排序好的数组里面删除重复的元素。

首先我们需要知道，对于一个排好序的数组来说，`A[N + 1] >= A[N]`，我们仍然使用两个游标i和j来处理，假设现在i = j + 1，如果A[i] == A[j]，那么我们递增i，直到A[i] != A[j]，这时候我们再设置A[j + 1] = A[i]，同时递增i和j，重复上述过程直到遍历结束。

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

譬如一个数组为1，1，2，3，首先i = 1，j = 0，这时候A[i] = A[j]，于是递增i，碰到2，不等于1，此时设置A[j + 1] = A[i]，也就是A[1] = A[2]，递增i和j为3和1，这时候A[3] != A[1]，设置A[j + 1] = A[i]，也就是A[2] = A[3]，再次递增，遍历结束。这时候新的数组长度就为2 + 1，也就是3。

# Remove Duplicates from Sorted Array II

> Follow up for "Remove Duplicates":
> What if duplicates are allowed at most twice?

> For example,
> Given sorted array A = [1,1,1,2,2,3],

> Your function should return length = 5, and A is now [1,1,2,2,3].

紧接着上一题，同样是移除重复的元素，但是可以允许最多两次重复元素存在。

仍然是第一题的思路，但是我们需要用一个计数器来记录重复的次数，如果重复次数大于等于2，我们会按照第一题的方式处理，如果不是重复元素了，我们将计数器清零。

代码如下：

```c++
class Solution {
public:
    int removeDuplicates(int A[], int n) {
        if(n == 0) {
            return 0;
        }

        int j = 0;
        int num = 0;
        for(int i = 1; i < n; i++) {
            if(A[j] == A[i]) {
                num++;
                if(num < 2) {
                    A[++j] = A[i];
                }
            } else {
                A[++j] = A[i];
                num = 0;
            }
        }
        return j + 1;
    }
};
```
