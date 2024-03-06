# 62.不同的路径

[题目链接](https://leetcode.cn/problems/unique-paths/description/)

### 思路

> 动态规划。容易识别出来是动态规划的题目，(i,j)指机器人处在从上往下从左往右的第i行第j列，dp\[i][j]是指到这个位置的不同的路径数量，那么这样状态转移方程很清楚了，具体见代码。看到状态转移方程，可以进一步优化空间，将二维表优化为一维数组即可。

### 时空复杂度

> O(n*m),S(n)

### 源码

```
class Solution {
public:
    int uniquePaths(int m, int n) {
        // dp[i][j]=dp[i-1][j]+dp[i][j-1]
        vector<int> dp(n, 0);
        for (int i = 1; i <= m; i++) {
            dp[0] = 1;
            for (int j = 1; j < n; j++) {
                dp[j] = dp[j] + dp[j - 1];
            }
        }
        return dp[n - 1];
    }
};
```

# 63.不同的路径II

[题目链接](https://leetcode.cn/problems/unique-paths-ii/description/)

### 思路

> DP法。与62.不同路径解答思路类似，稍微有些改动，注意代码精简的技巧。

### 时空复杂度

> O(n*m),S(n)

### 源码

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>> &obstacleGrid) {
        // dp[i][j]=dp[i-1][j]+dp[i][j-1]
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<int> dp(n, 0);
        dp[0] = 1;
        for (int i = 0; i < m; i++) {
            if (obstacleGrid[i][0] == 1) {
                dp[0] = 0;
            }
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    dp[j] = 0;
                } else {
                    dp[j] = dp[j] + dp[j - 1];
                }
            }
        }
        return dp[n - 1];
    }
};
```

