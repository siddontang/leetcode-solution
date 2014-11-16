# Find Minimum in Rotated Sorted Array

> Suppose a sorted array is rotated at some pivot unknown to you beforehand.

> (i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

> Find the minimum element.

> You may assume no duplicate exists in the array.

这题要求在一个轮转了的排序数组里面找到最小值，我们可以用二分法来做。

首先我们需要知道，对于一个区间A，如果A[start] < A[stop]，那么该区间一定是有序的了。

假设在一个轮转的排序数组A，我们首先获取中间元素的值，A[mid]，mid = (start + stop) / 2。因为数组没有重复元素，那么就有两种情况：

+ A[mid] > A[start]，那么最小值一定在右半区间，譬如[4,5,6,7,0,1,2]，中间元素为7，7 > 4，最小元素一定在[7,0,1,2]这边，于是我们继续在这个区间查找。
+ A[mid] < A[start]，那么最小值一定在左半区间，譬如[7,0,1,2,4,5,6]，这件元素为2，2 < 7，我们继续在[7,0,1,2]这个区间查找。

代码如下:

```c++
class Solution {
public:
    int findMin(vector<int> &num) {
        int size = num.size();

        if(size == 0) {
            return 0;
        } else if(size == 1) {
            return num[0];
        } else if(size == 2) {
            return min(num[0], num[1]);
        }

        int start = 0;
        int stop = size - 1;

        while(start < stop - 1) {
            if(num[start] < num[stop]) {
                return num[start];
            }

            int mid = start + (stop - start) / 2;
            if(num[mid] > num[start]) {
                start = mid;
            } else if(num[mid] < num[start]) {
                stop = mid;
            }
        }

        return min(num[start], num[stop]);
    }
};
```

# Find Minimum in Rotated Sorted Array

> Suppose a sorted array is rotated at some pivot unknown to you beforehand.

> (i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

> Find the minimum element.

> The array may contain duplicates.

这题跟上题唯一的区别在于元素可能有重复，我们仍然采用上面的方法，只是需要处理mid与start相等这种额外情况。

+ A[mid] > A[start]，右半区间查找。
+ A[mid] < A[start]，左半区间查找。
+ A[mid] = A[start]，出现这种情况，我们跳过start，重新查找，譬如[2,2,2,1]，A[mid] = A[start]都为2，这时候我们跳过start，使用[2,2,1]继续查找。

代码如下:

```c++
class Solution {
public:
    int findMin(vector<int> &num) {
        int size = num.size();

        if(size == 0) {
            return 0;
        } else if(size == 1) {
            return num[0];
        } else if(size == 2) {
            return min(num[0], num[1]);
        }

        int start = 0;
        int stop = size - 1;

        while(start < stop - 1) {
            if(num[start] < num[stop]) {
                return num[start];
            }

            int mid = start + (stop - start) / 2;
            if(num[mid] > num[start]) {
                start = mid;
            } else if(num[mid] < num[start]) {
                stop = mid;
            } else {
                start++;
            }
        }

        return min(num[start], num[stop]);
    }
};
```

这题需要注意，如果重复元素很多，那么最终会退化到遍历整个数组，而不是二分查找了。
