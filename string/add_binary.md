# Add Binary

> Given two binary strings, return their sum (also a binary string).

> For example,
a = "11"
b = "1"
Return "100".

> 题目翻译: 对于给定的两个二进制数字所表达的字符串，我们求其相加所得到的结果， 根据上例便可得到答案.

> 题目分析: 我认为这道题所要注意的地方涵盖以下几个方面:

1. 对字符串的操作.
2. 对于加法，我们应该建立一个进位单位，保存进位数值.
3. 我们还要考虑两个字符串如果不同长度会怎样.
4. int 类型和char类型的相互转换.

> 时间复杂度:其实这就是针对两个字符串加起来跑一遍，O(n) n代表长的那串字符串长度.

代码如下:

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        int len1 = a.size();
        int len2 = b.size();
        if(len1 == 0)
            return b;
        if(len2 == 0)
            return a;

        string ret;
        int carry = 0;
        int index1 = len1-1;
        int index2 = len2-1;

        while(index1 >=0 && index2 >= 0)
        {
            int num = (a[index1]-'0')+(b[index2]-'0')+carry;
            carry = num/2;
            num = num%2;
            index1--;
            index2--;
            ret.insert(ret.begin(),num+'0');
        }

        if(index1 < 0&& index2 < 0)
        {
            if(carry == 1)
            {
                ret.insert(ret.begin(),carry+'0');
                return ret;
            }
        }

        while(index1 >= 0)
        {
            int num = (a[index1]-'0')+carry;
            carry = num/2;
            num = num%2;
            index1--;
            ret.insert(ret.begin(),num+'0');
        }
        while(index2 >= 0)
        {
            int num = (b[index2]-'0')+carry;
            carry = num/2;
            num = num%2;
            index2--;
            ret.insert(ret.begin(),num+'0');
        }
        if(carry == 1)
            ret.insert(ret.begin(),carry+'0');
        return ret;
    }
};

```


