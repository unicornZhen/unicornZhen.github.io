## 647.回文子串

[题目链接](https://leetcode.cn/problems/palindromic-substrings/description/)

### 思路

> 动态规划。
>
> 方法一：dp\[i][j]表示s的从索引i到索引j的子串是否为回文字符串，动态转移方程见源代码，比较简单。
>
> 方法二：回文中心。经过分析可以得到，找出所有的回文中心，及相对应的回文串的最大长度。
>
> 方法三：Manacher,可以在O(n)的时间复杂度找s的最大回文串，对这个算法有点印象即可，会用即可，不要求理解和掌握。
>
> 方法一是我想出来的，方法二和三是[leetcode题解](https://leetcode.cn/problems/palindromic-substrings/solutions/379987/hui-wen-zi-chuan-by-leetcode-solution/)，注意方法二的做法，空间复杂度为S(1)

### 时空复杂度

> 方法一：$$O(n^2),S(n^2)$$
>
> 方法二：$$O(n^2),S(1)$$

### 源码

**方法一**

```C++
class Solution {
public:
    int countSubstrings(string s) {
        int len = (int) s.length();
        int result = len;
        // 预处理s使得判断s任意两个位置之间的字符是否为回文串时间复杂度为O(1)
        vector<vector<bool>> dp(len, vector<bool>(len, true));
        for (int i = 0; i < len - 1; i++) {
            dp[i][i + 1] = (s[i] == s[i + 1]);
            if (dp[i][i + 1]) {
                result++;
            }
        }
        for (int d = 2; d < len; d++) {
            for (int i = 0; i < len - d; i++) {
                if (s[i] == s[i + d]) {
                    dp[i][i + d] = dp[i + 1][i + d - 1];
                } else {
                    dp[i][i + d] = false;
                }
                if (dp[i][i + d]) {
                    result++;
                }
            }
        }
        return result;
    }
};
```

**方法二**

```C++
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size(), ans = 0;
        for (int i = 0; i < 2 * n - 1; ++i) {
            // 统一处理了奇数和偶数的回文中心的情况
            int l = i / 2, r = i / 2 + i % 2;
            while (l >= 0 && r < n && s[l] == s[r]) {
                --l;
                ++r;
                ++ans;
            }
        }
        return ans;
    }
};
```



## 516.最长回文子序列

[516. 最长回文子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-subsequence/)

### 思路

> 动态规划。与647.回文子串类似，具体见源代码。

### 时空复杂度

> $$O(n^2),S(n^2)$$

### 源码

```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int len = (int) s.length();
        int result = 1;

        // dp[i][j]表示s[i:j]的最长回文子序列
        vector<vector<int>> dp(len, vector<int>(len));
        for (int i = 0; i < len - 1; i++) {
            dp[i][i] = 1;
            dp[i][i + 1] = (s[i] == s[i + 1] ? 2 : 1);
            result=max(dp[i][i+1],result);
        }
        dp[len - 1][len - 1] = 1;

        for (int d = 2; d < len; d++) {
            for (int i = 0; i < len - d; i++) {
                if (s[i] == s[i + d]) {
                    dp[i][i + d] = dp[i + 1][i + d - 1] + 2;
                } else {
                    dp[i][i + d] = max(dp[i + 1][i + d], dp[i][i + d - 1]);
                }
                result = max(dp[i][i + d], result);
            }
        }

        return result;
    }
};
```

