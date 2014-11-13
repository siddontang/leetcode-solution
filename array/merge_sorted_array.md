# Merge Sorted Array

> Given two sorted integer arrays A and B, merge B into A as one sorted array.

> Note:
> You may assume that A has enough space (size that is greater or equal to m + n) to hold additional elements from B. The number of elements initialized in A and B are m and n respectively.

A和B都已经是排好序的数组，我们只需要从后往前比较就可以了。

因为A有足够的空间容纳A + B，我们使用游标i指向m + n - 1，也就是最大数值存放的地方，从后往前遍历A，B，谁大就放到i这里，同时递减i。

代码如下：

```c++
class Solution {
public:
    void merge(int A[], int m, int B[], int n) {
        int i = m + n - 1;
        int j = m - 1;
        int k = n - 1;

        while(i >= 0) {
            if(j >= 0 && k >= 0) {
                if(A[j] > B[k]) {
                    A[i] = A[j];
                    j--;
                } else {
                    A[i] = B[k];
                    k--;
                }
            } else if(j >= 0) {
                A[i] = A[j];
                j--;
            } else if(k >= 0) {
                A[i] = B[k];
                k--;
            }
            i--;
        }
    }
};
```
