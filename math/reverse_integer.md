# Reverse Integer

> Reverse Integer: Reverse digits of an integer.

> Example1: x = 123, return 321.
> Example: x = -123, return -321.


> 题目翻译: 反转一个数字，比如123要反转为321，-123反转为-321.

> 题目解析: 这是一个纯数学问题，我们要考虑到corner case的条件，也就是说如果这个数字是0的话，我们直接就返回这个数字就可以了.很简单的问题，直接上代码吧:


```c++
class Solution {
public:
    int reverse(int x) {
        if(x == 0)
            return x;
        int ret = 0;
        while(x!=0)
        {
            ret = ret*10 + x%10;
            x = x/10;
        }
        return ret;
    }
};

```
