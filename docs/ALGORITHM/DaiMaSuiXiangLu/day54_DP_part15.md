# 392.判断子序列

[题目链接](https://leetcode.cn/problems/is-subsequence/description/)

### 思路

> 方法一：贪心思想、双指针法。从前往后扫描s和t,尽可能匹配s中的字符。
>
> 方法二：动态规划。dp\[i][j]表示s前i个字符是否是t前j个字符的子序列，动态转移方程与part14中的两个题目类似。
>
> 本题采用方法一解答，最为简单。

### 时空复杂度

> n为t的长度
>
> O(n),S(1)

### 源码

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int i_s = 0;
        int i_t = 0;
        while (i_s < s.size() && i_t < t.size()) {
            if (s[i_s] == t[i_t]) {
                i_s++;
            }
            i_t++;
        }
        return i_s >= s.size();
    }
};
```

# 115.不同的子序列

[题目链接](https://leetcode.cn/problems/distinct-subsequences/description/)

### 思路

> 动态规划。常见的子序列题目，dp表的设计和动态转移方程与参照一般的子序列题目。

### 时空复杂度

> 设n为s的长度，m为t的长度
>
> O(mn),S(mn)

### 源码

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        int len1 = (int) s.length();
        int len2 = (int) t.length();

        // dp[i][j]表示t的前j-1个字符是s的前i-1个字符的不同的子序列个数
        vector<vector<unsigned long long>> dp(len1 + 1, vector<unsigned long long>(len2 + 1, 0));
        for (int i = 0; i <= len1; i++) {
            dp[i][0] = 1;
        }
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (s[i - 1] == t[j - 1]) {
                    // s[i-1]与t[j-1]匹配+s[i-1]不与t[j-1]匹配
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else {
                    // s[i-1]不与t[j-1]匹配
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        return dp[len1][len2];
    }
};
```

