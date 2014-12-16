# Search for a Range

> Given a sorted array of integers, find the starting and ending position of a given target value.

> Your algorithm's runtime complexity must be in the order of O(log n).

> If the target is not found in the array, return [-1, -1].

> For example,

> Given [5, 7, 7, 8, 8, 10] and target value 8,

> return [3, 4].

这题要求在一个排好序可能有重复元素的数组里面找到包含某个值的区间范围。要求使用O(log n)的时间，所以我们采用两次二分查找。首先二分找到第一个该值出现的位置，譬如m，然后在[m, n)区间内第二次二分找到最后一个该值出现的位置。代码如下：

```c++
class Solution {
public:
    vector<int> searchRange(int A[], int n, int target) {
       if(n == 0) {
            return vector<int>({-1, -1});
        }

        vector<int> v;
        int low = 0;
        int high = n - 1;
        //第一次二分找第一个位置
        while(low <= high) {
            int mid = low + (high - low) / 2;
            if(A[mid] >= target) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }

        if(low < n && A[low] == target) {
            v.push_back(low);
        } else {
            return vector<int>({-1, -1});
        }

        low = low;
        high = n - 1;
        //从第一个位置开始进行第二次二分，找最后一个位置
        while(low <= high) {
            int mid = low + (high - low) / 2;
            if(A[mid] <= target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        
        v.push_back(high);
        return v;
    }
};
```
