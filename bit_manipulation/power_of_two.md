# Power of Two

> Given an integer, write a function to determine if it is a power of two.


题目翻译: 
给出一个整数，判断它是否是2的幂。

题目分析: 
2的整数次幂对应的二进制数只含有0个或者1个1，所以我们要做的就是判断输入的数的二进制表达形式里是否符合这一条件。
有一种corner case需要注意，当输入的数为负数的时候，一定不是2的幂。

时间复杂度：O(n)
空间复杂度：O(1)

代码如下:

```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
       if (n < 0) return false;
       bool hasOne = false;
       while (n > 0) {
           if (n & 1) {
               if (hasOne) {
                   return false;
               }
               else {
                   hasOne = true;
               }
           }
           n >>= 1;
       }
       return hasOne;
    }
};
```


