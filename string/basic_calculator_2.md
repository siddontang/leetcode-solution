# Basic Calculator II

> Implement a basic calculator to evaluate a simple expression string.

> The expression string contains only non-negative integers, ```+, -, *, /``` operators and empty spaces ``` ``` . The integer division should truncate toward zero.

> You may assume that the given expression is always valid.

> Some examples:
```
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5
```
> Note: Do not use the ```eval``` built-in library function.

题目翻译：
实现一个简易的计算器来对简单的字符串表达式求值。
字符串表达式只包含非负整数，+，-，*，/四种运算符，以及空格。整数除法向零取整。
给出的表达式都是有效的。
不要使用内置的eval函数。

题目分析：
通常对算术表达式求值都是用栈来实现的，但是鉴于本题的情形比较简单，所以可以不用栈来实现。
总体思路是，依次读入字符串里的字符，遇到符号的时候就进行运算。如果是乘除法，就把结果存入中间变量，如果是加减法就把结果存入最终结果。

用C++实现的时候，可以在循环中使用```string```类的```find_first_not_of```方法来跳过空格。
读到数字时，继续向后读，直到不是数字的字符，或者超出字符串长度为止。

代码如下：
```c++
class Solution {
public:
    int calculate(string s) {
    	int result = 0, inter_res = 0, num = 0;
    	char op = '+';
    	char ch;
    	for (int pos = s.find_first_not_of(' '); pos < s.size(); pos = s.find_first_not_of(' ', pos)) {
    		ch = s[pos];
    		if (ch >= '0' && ch <= '9') {
    			int num = ch - '0';
    			while (++pos < s.size() && s[pos] >= '0' && s[pos] <= '9')
    				num = num * 10 + s[pos] - '0';
    			switch (op) {
    			case '+':
    				inter_res += num;
    				break;
    			case '-':
    				inter_res -= num;
    				break;
    			case '*':
    				inter_res *= num;
    				break;
    			case '/':
    				inter_res /= num;
    				break;
    			}
    		}	
    		else {
    			if (ch == '+' || ch == '-') {
    				result += inter_res;
    				inter_res = 0;
    			}
    			op = s[pos++];
    		}
    	}
    	return result + inter_res;
    }
};
```