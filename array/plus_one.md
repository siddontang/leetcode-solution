# Plus One

> Given a non-negative number represented as an array of digits, plus one to the number.

> The digits are stored such that the most significant digit is at the head of the list.

这道题目很简单，就是考的加法进位问题。直接上代码：

```c++
class Solution {
public:
    vector<int> plusOne(vector<int> &digits) {
        vector<int> res(digits.size(), 0);
        int sum = 0;
        int one = 1;
        for(int i =  digits.size() - 1; i >= 0; i--) {
            sum = one + digits[i];
            one = sum / 10;
            res[i] = sum % 10;
        }

        if(one > 0) {
            res.insert(res.begin(), one);
        }
        return res;
    }
};
```

