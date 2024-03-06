# 518.零钱和II

[题目链接](https://leetcode.cn/problems/coin-change-ii/description/)

### 思路

> 动态规划。属于完全背包这类问题，与0-1背包相区分，注意其状态转移方程dp\[i][j]=dp\[i-1][j]+dp\[i][j-coin]与0-1背包的dp\[i][j]=dp\[i-1][j]+dp\[i-1][j-coin]相区分。

### 时空复杂度

> O(amount*n),S(amount)

### 源码

```
class Solution {
public:
    int change(int amount, vector<int> &coins) {
        vector<long long> dp(amount + 1, 0);
        dp[0] = 1;
        for (auto coin: coins) {
            // dp[i][j]=dp[i-1][j]+dp[i][j-coin]
            for (int i = 0; i <= amount; i++) {
                dp[i] = dp[i];
                if (i >= coin) {
                    dp[i] += dp[i - coin];
                }
            }
        }
        return (int) dp[amount];
    }
};
```

# 题目

[题目链接]()

### 思路

> 

### 时空复杂度

> 

### 源码

```

```

