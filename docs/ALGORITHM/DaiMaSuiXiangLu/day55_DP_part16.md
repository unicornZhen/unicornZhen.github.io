# 583.两个字符串的删除操作

[题目链接](https://leetcode.cn/problems/delete-operation-for-two-strings/description/)

### 思路

> 动态规划。分析题目可以发现，这个题目可以转化为求两个字符串的最长公共子序列（1143.最长公共子序列）。
>
> 也可以直接分析题目采用动态规划做，在这里为了方便简单起见，故采用求两个字符串的最长公共子序列的方式。

### 时空复杂度

> 设m为word1的长度，n为word2的长度
>
> O(nm)，S(nm)

### 源码

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int len1 = (int) word1.length();
        int len2 = (int) word2.length();
        // dp[i][j]表示text1前i个字符和text2前j个字符的最长公共子序列
        // dp[i][j]的状态转移分为以text1[i]和text2[j]结尾的最长公共子序列和不以它为结尾的最长公共子序列
        // 可以使用滚动数组将空间复杂度优化至O(n)
        vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1, 0));
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return (len1-dp[len1][len2])+(len2-dp[len1][len2]);
    }
};
```

# 72.编辑距离

[题目链接](https://leetcode.cn/problems/edit-distance/description/)

### 思路

> 动态规划。这种题目容易识别出是动态规划的题目，具有明显的子问题的特征，且无后效性，dp\[i][j]表示word1前i个字符和word2的前j个字符的距离。推导dp的状态转移方程的关键点是将insert和delete、replace三种操作等价于delete和replace两种操作，原因不赘述(好像也不需要这种转化），具体的状态转移方程见源码。

### 时空复杂度

> 设n为word1的长度，m为word2的长度
>
> O(nm)，S(nm)

### 源码

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int len1 = (int) word1.length();
        int len2 = (int) word2.length();

        // dp[i][j]表示word1前i个和word2前j个字符的编辑距离
        // dp[i][j]=
        vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1));
        for (int i = 0; i <= len1; i++) {
            dp[i][0] = i;
        }
        for (int i = 0; i <= len2; i++) {
            dp[0][i] = i;
        }
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                // 针对word1[i-1]与word2[j-1]的操作，要么去掉或者变化要么相同
                if (word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = min(dp[i - 1][j - 1], min(dp[i - 1][j], dp[i][j - 1])) + 1;
                }
            }
        }
        return dp[len1][len2];
    }
};
```

## 总结

* 注意编辑距离的定义和解法，这个要记住，其余就是子序列类问题的状态转移方程的一般思考方向，是否需要二维数组等等。
