# Palindrome Number

> Determine whether an integer is a palindrome. Do this without extra space.

题目翻译:
给定一个数字，要求判断这个数字是否为回文数字. 比如121就是回文数字，122就不是回文数字.

解题思路:
这道题很明显是一道数学题,计算一个数字是否是回文数字，我们其实就是将这个数字除以10，保留他的余数，下次将余数乘以10，加上这个数字再除以10的余数.

需要注意的点:
1. 负数不是回文数字.
2. 0是回文数字.

时间复杂度:
logN

代码如下:

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0)
            return false;
        else if(x == 0)
            return true;
        else
        {
            int tmp = x;
            int y = 0;
            while(x != 0)
            {
                y = y*10 + x%10;
                x = x/10;
            }
            if(y == tmp)
                return true;
            else
                return false;
        }
    }
};
```


