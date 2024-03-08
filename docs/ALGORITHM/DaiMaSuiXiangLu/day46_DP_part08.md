# 139.单词拆分

[题目链接](https://leetcode.cn/problems/word-break/description/)

### 思路

> 动态规划。完全背包的求排列问题，可以参考337.组合总和的思路。

### 时空复杂度

> 设m为s的长度，n为wordDict的长度。
>
> 时间复杂度不便于计算，因为需要比较wordDict中的单词是否与s的部分相匹配，时间复杂度不便计算，空间复杂度为O(m)

### 源码

```C++
class Solution {
public:
    bool wordBreak(string s, vector<string> &wordDict) {
        int len=s.size();
        vector<bool> dp(len+1, false);
        dp[0] = true;
        for(int i=0;i<=len;i++){
            for(auto &word:wordDict){
                // dp[i][j]=dp[i-1][j]+dp[
                if(i>=word.size()&&s.substr(i-word.size(),word.size())==word){
                    dp[i]=dp[i-word.size()]||dp[i];
                }
            }
        }
        return dp[len];
    }
};
```

## 总结

* 背包类问题的识别，0-1背包、完全背包、多重背包，以及从完全背包衍生出来的求排列的题目，例如377.组合总和、70.爬楼梯的完全背包解法等
* 注意先是完整的dp表，再考虑空间优化，dp的合法性从数学归纳法得到支撑
