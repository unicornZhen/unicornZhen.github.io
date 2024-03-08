# 309.买卖股票的最佳时机

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

### 思路

> 动态规划。属于买卖股票这类问题，状态在122.买卖股票上进行变化，共计三种。具体见源代码。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len=(int)prices.size();
        if(len<1){
            return 0;
        }
        // dp[i][0]：持有股票
        // dp[i][1]：不持有股票且是刚完成一笔交易
        // dp[i][2]: 不持有股票且不是刚完成一笔交易
        vector<vector<int>> dp(len,vector<int>(3,0));
        dp[0][0]=-prices[0];
        dp[0][1]=0;
        dp[0][2]=0;
        
        for(int i=1;i<len;i++){
            dp[i][0]=max(dp[i-1][0],dp[i-1][2]-prices[i]);
            dp[i][1]=dp[i][0]+prices[i];
            dp[i][2]=max(dp[i-1][2],dp[i-1][1]);
        }
        return max(dp[len-1][1],dp[len-1][2]);
    }
};
```

# 714.买卖股票的最佳时机含手续费

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

### 思路

> 动态规划。属于股票买卖这类问题，与122.股票买卖的最佳时机基本一样。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    int maxProfit(vector<int> &prices, int fee) {
        int len = (int) prices.size();
        if (len < 1) {
            return 0;
        }
        // dp[i][0]:持有股票
        // dp[i][1]：不持有股票
        vector<vector<int>> dp(len, vector<int>(2, 0));
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i][0] + prices[i] - fee);
        }
        return dp[len - 1][1];
    }
};
```

## 总结

* 区分好股票买卖的状态定义，灵活运用数学归纳法和生活的实际情况
