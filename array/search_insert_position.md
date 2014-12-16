# Search Insert Position

> Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

> You may assume no duplicates in the array.

> Here are few examples.

> [1,3,5,6], 5 → 2

> [1,3,5,6], 2 → 1

> [1,3,5,6], 7 → 4

> [1,3,5,6], 0 → 0

这题要求在一个排好序的数组查找某值value，如果存在则返回对应index，不存在则返回能插入到数组中的index（保证数组有序）。

对于不存在的情况，我们只需要在数组里面找到最小的一个值大于value的index，这个index就是我们可以插入的位置。譬如[1, 3, 5, 6]，查找2，我们知道3是最小的一个大于2的数值，而3的index为1，所以我们需要在1这个位置插入2。如果数组里面没有值大于value，则插入到数组末尾。

我们采用二分法解决：

```c++
class Solution {
public:
    int searchInsert(int A[], int n, int target) {
        int low = 0;
        int high = n - 1;

        while(low <= high) {
            int mid = low + (high - low) / 2;
            if(A[mid] == target) {
                return mid;
            } else if(A[mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return low;
    }
};
```
