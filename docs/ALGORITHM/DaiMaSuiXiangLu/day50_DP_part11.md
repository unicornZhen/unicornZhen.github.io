# 123.买卖股票的最佳时机III

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/description/)

### 思路

> 动态规划。属于股票买卖这一类问题，状态定义与状态转移与121.股票买卖类似。

### 时空复杂度

> O(n),S(n)(可以优化至O(1))

### 源码

```C++
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int len = (int) prices.size();
        if (len < 1) {
            return 0;
        }
        // dp[i][0]表示买了第一支股票的最大值
        // dp[i][1]表示完成第一笔交易的最大值
        // dp[i][2]表示买了第二支股票的最大值
        // dp[i][3]表示完成第二笔股票交易的最大值
        vector<vector<int>> dp(len, vector<int>(4, 0));
        dp[0][0] = -prices[0];
        // 买入又卖出
        dp[0][1] = 0;
        // 买入、卖出、买入
        dp[0][2] = -prices[0];
		// 买入、卖出、买入、卖出
		dp[0][3] = 0;
        for (int i = 1; i < len; i++) {
        	// 以下的状态转移方程包括当天买入、卖出的情况
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
            dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] - prices[i]);
            dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] + prices[i]);
        }
        return max(dp[len - 1][3], dp[len - 1][1]);
    }
};
```

# 188.买卖股票的最佳时机IV

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)

### 思路

> 动态规划。是123.买卖股票的最佳时机III的推广，解法也是题123的解法的一般化,具体解法见源代码。

### 时空复杂度

> O(n*k)，S(n*k)(空间复杂度可以进一步优化至O(k))

### 源码

```C++
class Solution {
public:
    int maxProfit(int k, vector<int> &prices) {
        int len = (int) prices.size();
        int result = 0;
        if (len < 1) {
            return 0;
        }
        // dp[i][j]：如果j为负数则表示买了第一支股票的最大值;如果j为偶数表示完成第一笔交易的最大值
        vector<vector<int>> dp(len, vector<int>(2 * k, 0));
        for (int i = 0; i < k; i++) {
            dp[0][2 * i] = -prices[0];
        }
        for (int i = 1; i < len; i++) {
            // 以下的状态转移方程包括当天买入、卖出的情况
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
            for (int j = 1; j < k; j++) {
                dp[i][2 * j] = max(dp[i - 1][2 * j], dp[i - 1][2 * j - 1] - prices[i]);
                dp[i][2 * j + 1] = max(dp[i - 1][2 * j + 1], dp[i - 1][2 * j] + prices[i]);
            }
        }
        for (int i = 0; i < k; i++) {
            result = max(dp[len - 1][2 * i + 1], result);
        }
        return result;
    }
};
```

## 总结

* 注意188.买卖股票的最佳时机IV中买卖k笔交易的解法，k为2时就为123的解法，k为n时就是122.买卖股票II的解法。
