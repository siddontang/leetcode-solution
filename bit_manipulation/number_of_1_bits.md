# Number of 1 Bits

> Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).
> For example, the 32-bit integer `11` has binary representation `00000000000000000000000000001011`, so the function should return 3.


题目翻译: 
给出一个整数，求它包含二进制1的位数。例如，32位整数`11`的二进制表达形式是`00000000000000000000000000001011`，那么函数应该返回3。

题目分析: 
设输入的数为n，把n与1做二进制的与(AND)运算，即可判断它的最低位是否为1。如果是的话，把计数变量加一。然后把n向右移动一位，重复上述操作。当n变为0时，终止算法，输出结果。

时间复杂度：O(n)
空间复杂度：O(1)

代码如下:

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n > 0) {
            count += n & 1;
            n >>= 1;
        }
        return count;
    }
};
```


