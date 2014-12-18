# Find Peak Element

> A peak element is an element that is greater than its neighbors.

> Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.

> The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

> You may imagine that num[-1] = num[n] = -∞.

> For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.

这题要求我们在一个无序的数组里面找到一个peak元素，所谓peak，就是值比两边邻居大就行了。

对于这题，最简单地解法就是遍历数组，只要找到第一个元素，大于两边就可以了，复杂度为O(N)。但这题还可以通过二分来做。

首先我们找到中间节点mid，如果大于两边返回当前index就可以了，如果左边的节点比mid大，那么我们可以继续在左半区间查找，这里面一定存在一个peak，为什么这么说呢？假设此时的区间范围为[0, mid - 1]， 因为num[mid - 1]一定大于num[mid]了，如果num[mid - 2] <= num[mid - 1]，那么num[mid - 1]就是一个peak。如果num[mid - 2] > num[mid - 1]，那么我们就继续在[0, mid - 2]区间查找，因为num[-1]为负无穷，所以最终我们绝对能在左半区间找到一个peak。同理右半区间一样。

代码如下：

```c++
class Solution {
public:
    int findPeakElement(const vector<int> &num) {
        int n = num.size();
        if(n == 1) {
            return 0;
        }

        int start = 0;
        int end = n - 1;
        int mid = 0;

        while(start <= end) {
            mid = start + (end - start) / 2;
            if((mid == 0 || num[mid] >= num[mid - 1]) &&
               (mid == n - 1 || num[mid] >= num[mid + 1])) {
                    return mid;
            }else if(mid > 0 && num[mid-1] > num[mid]) {
                end = mid - 1;
            } else {
                start = mid + 1;
            }
        }
        return mid;
    }
};
```
