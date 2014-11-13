# Remove Element

> Given an array and a value, remove all instances of that > value in place and return the new length.

> The order of elements can be changed. It doesn't matter what you leave beyond the new length.

作为开胃菜，我当然选取了最容易的一道题目，在一个数组里面移除指定value，并且返回新的数组长度。这题唯一需要注意的地方在于`in place`，不能新建另一个数组。

方法很简单，使用两个游标i，j，遍历数组，如果碰到了value，使用j记录位置，同时递增i，直到下一个非value出现，将此时i对应的值复制到j的位置上，增加j，重复上述过程直到遍历结束。这时候j就是新的数组长度。

代码如下：

```c++
class Solution {
public:
    int removeElement(int A[], int n, int elem) {
        int i = 0;
        int j = 0;
        for(i = 0; i < n; i++) {
            if(A[i] == elem) {
                continue;
            }

            A[j] = A[i];
            j++;
        }

        return j;
    }
};
```

举一个最简单的例子，譬如数组为1，2，2，3，2，4，我们需要删除2，首先初始化i和j为0，指向第一个位置，因为第一个元素为1，所以A[0] = A[0]，i和j都加1，而第二个元素为2，我们递增i，直到碰到3，此时A[1] = A[3]，也就是3，递增i和j，这时候下一个元素又是2，递增i，直到碰到4，此时A[2] = A[5]，也就是4，再次递增i和j，这时候数组已经遍历完毕，结束。这时候j的值为3，刚好就是新的数组的长度。
