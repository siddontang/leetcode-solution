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
        vector<bool> dp(len + 1, false);
        dp[0] = true;

        for(int i = 1; i <= len; i++) {
            for(int j = i - 1; j >= 0; j--) {
                if(dp[j] && dict.find(s.substr(j, i - j)) != dict.end()) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[len];
    }
};
```

# World Break II

> Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

> Return all such possible sentences.

> For example, given
s = "catsanddog",
dict = ["cat", "cats", "and", "sand", "dog"].

> A solution is ["cats and dog", "cat sand dog"].

这道题不同于上一题，需要我们得到所有能切分的解。这道题难度很大，我们需要采用dp + dfs的方式求解，首先根据dp得到该字符串能否被切分，同时在dp的过程中记录属于字典的子串信息，供后续dfs使用。

首先我们使用dp[i][j]表示起始索引为i，长度为j的子串能否被切分，它有三种状态:

1. dp[i][j] = true && dp[i][j] in dict，这种情况是这个子串直接在字典中
2. dp[i][j] = true && dp[i][j] not in dict，这种情况是这个子串不在字典中，但是它能切分成更小的子串，而这些子串在字典中
3. dp[i][j] = false，子串不能被切分

根据题意，我们需要求出所有切分的解，所以在进行dp的时候需要处理1和2这两种情况，因为对于2来说，dp[i][j]是要继续被切分的，也就是说我们只需要关注第1种情况的子串。

当dp完成之后，我们就需要使用dfs来得到整个的解。在dp[i][j] = 1的情况下面，我们只需要dfs递归处理后面i + j开始的子串就可以了。

代码如下：

```c++

class Solution {
public:
    vector<vector<char> >dp;
    vector<string> vals;
    string val;
    
    vector<string> wordBreak(string s, unordered_set<string> &dict) {
        int len = (int)s.size();
        dp.resize(len);
        for(int i = 0; i < len; i++) {
            dp[i].resize(len + 1, 0);
        }

        for(int i = 1; i <= len; i++) {
            for(int j = 0; j < len -i + 1; j++) {
                //直接存在于字典中，是第1种情况
                if(dict.find(s.substr(j, i)) != dict.end()) {
                    dp[j][i] = 1;
                    continue;
                }
                //如果不存在，则看子串是不是能被切分，这是第2中情况
                for(int k = 1; k < i && k < len -j; k++) {
                    if(dp[j][k] && dp[j + k][i - k]) {
                        dp[j][i] = 2;
                        break;
                    }
                }
            }
        }

        //不能切分，不用dfs了
        if(dp[0][len] == 0) {
            return vals;
        }

        dfs(s, 0);
        return vals;
    }

    void dfs(const string& s, int start) {
        int len = (int)s.size();
        if(start == len) {
            vals.push_back(val);
            return;
        }

        for(int i = 1; i <= len - start;i++) {
            if(dp[start][i] == 1) {
                int oldLen = (int)val.size();
                if(oldLen != 0) {
                    val.append(" ");
                }
                val.append(s.substr(start, i));

                //我们从start + i开始继续dfs
                dfs(s, start + i);
                val.erase(oldLen, string::npos);
            }
        }
    }
};
```