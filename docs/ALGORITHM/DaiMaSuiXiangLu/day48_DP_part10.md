# 121.买卖股票的最佳时机

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

### 思路

> 方法一：贪心法。贪心法已在贪心章节做过，在此不赘述。
>
> 方法二：动态规划。适合股票类问题的一般性的动态规划解题思路，dp\[i][0]表示第i天持有股票所得最多现金,dp\[i][1] 表示第i天不持有股票所得最多现金。dp表的设计抓住了股票买卖的本质，这个要注意。
>
> 本题采用方法二解决问题。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
// 版本一
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        if (len == 0) return 0;
        vector<vector<int>> dp(len, vector<int>(2));
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
        }
        return dp[len - 1][1];
    }
};
```

# 122.买卖股票的最佳时机II

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

### 思路

> 方法一：贪心。贪心解法在贪心章节已介绍，在此不再赘述。
>
> 方法二：动态规划。与121.买卖股票类似，dp表和状态转移方程十分类似。

### 时空复杂度

> O(n),S(n)(空间可以进一步优化至O(1))

### 源码

```C++
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int len = (int) prices.size();
        if (len < 1) {
            return 0;
        }
        // dp[i][0]表示第i天持有股票的最大值,dp[i][0]=max(dp[i-1][1]-nums[i],dp[i-1][0])
        // dp[i][1]表示第i天不持有股票的最大值,dp[i][1]=max(dp[i-1][1],dp[i-1][0]+nums[i])
        vector<vector<int>> dp(len, vector<int>(2, 0));
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][1] - prices[i], dp[i - 1][0]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }

        return dp[len - 1][1];
    }
};
```

## 总结

* 注意股票买卖的DP表的设计
