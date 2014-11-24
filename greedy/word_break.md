# Word Break

> Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

> For example, given
s = "leetcode",
dict = ["leet", "code"].

> Return true because "leetcode" can be segmented as "leet code".

这题的意思是给出一本词典以及一个字符串，能否切分这个字符串使得每个字串都在字典里面存在。

假设dp(i)表示长度为i的字串能否别切分，dp方程如下:

`dp(i) = dp(j) && s[j, i) in dict, 0 <= j < i`

代码如下

```c++
class Solution {
public:
    bool wordBreak(string s, unordered_set<string> &dict) {
        int len = (int)s.size();
        vector<bool> words(len + 1, false);
        words[0] = true;

        for(int i = 1; i <= len; i++) {
            for(int j = i - 1; j >= 0; j--) {
                if(words[j] && dict.find(s.substr(j, i - j)) != dict.end()) {
                    words[i] = true;
                    break;
                }
            }
        }
        return words[len];
    }
};
```
