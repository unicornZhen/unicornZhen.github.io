# 70.爬楼梯

[题目链接](https://leetcode.cn/problems/climbing-stairs/description/)

### 思路

> 动态规划。有两种方法，方法一是最常见的方法，dp[i]为爬到第i阶楼梯的爬法数量。
>
> 方法二，转化为完全背包问题，设nums={1,2}，dp\[i][j]为使用nums的前j种爬法爬到第i阶楼梯的爬法数量。
>
> 本次采用方法二。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    int climbStairs(int n) {
        vector<int> nums{1, 2};
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (auto num: nums) {
                if (num <= i) {
                    dp[i] += dp[i - num];
                }
            }
        }

        return dp[n];
    }
};

```

# 322.零钱兑换

[题目链接](https://leetcode.cn/problems/coin-change/description/)

### 思路

> 动态规划。属于完全背包之类的问题，解题思路省略，与前面几个题目的解题思路一样。

### 时空复杂度

> O(amount*n),S(amount)

### 源码

```C++
class Solution {
public:
    int coinChange(vector<int> &coins, int amount) {
        const int INFIN = 10E4 + 1;
        vector<int> dp(amount + 1, INFIN);
        dp[0] = 0;
        // dp[i][j]=min{dp[i-1][j],dp[i][j-coin]+1}

        for (auto coin: coins) {
            for (int i = 1; i <= amount; i++) {
                if (i >= coin) {
                    dp[i] = min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        if (dp[amount] >= INFIN) {
            return -1;
        }
        return dp[amount];
    }
};
```

# 279.完全平方数

[题目链接](https://leetcode.cn/problems/perfect-squares/description/)

### 思路

> 动态规划。完全背包，与322.零钱兑换一样。

### 时空复杂度

> O($$n^{3/2}$$),S(n)

### 源码

```C++
class Solution {
public:
    int numSquares(int n) {
        const int INFI = 10e4 + 1;
        vector<int> nums;
        for (int i = 1; i * i <= n; i++) {
            nums.push_back(i * i);
        }

        vector<int> dp(n + 1, INFI);
        dp[0] = 0;
        for (auto num: nums) {
            for (int i = 1; i <= n; i++) {
                if (i >= num) {
                    dp[i] = min(dp[i], dp[i - num] + 1);
                }
            }
        }
        return dp[n];
    }
};
```

