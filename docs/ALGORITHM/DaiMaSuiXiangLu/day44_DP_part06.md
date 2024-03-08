# 518.零钱和II

[题目链接](https://leetcode.cn/problems/coin-change-ii/description/)

### 思路

> 动态规划。属于完全背包这类问题，与0-1背包相区分，注意其状态转移方程dp\[i][j]=dp\[i-1][j]+dp\[i][j-coin]与0-1背包的dp\[i][j]=dp\[i-1][j]+dp\[i-1][j-coin]相区分。

### 时空复杂度

> O(amount*n),S(amount)

### 源码

```C++
class Solution {
public:
    int change(int amount, vector<int> &coins) {
        vector<long long> dp(amount + 1, 0);
        dp[0] = 1;
        for (auto coin: coins) {
            // dp[i][j]=dp[i-1][j]+dp[i][j-coin]
            for (int i = 0; i <= amount; i++) {
                if (i >= coin) {
                    dp[i] += dp[i - coin];
                }
            }
        }
        return (int) dp[amount];
    }
};
```

# 377.组合总合IV

[题目链接](https://leetcode.cn/problems/combination-sum-iv/description/)

### 思路

> 动态规划。属于完全背包的问题，但动态规划的转移方程和遍历顺序还是有点难以想到，解决了求排列的问题，具体解决机制比较新颖，这个记住即可，解答过程可以参见[leetcode解答](https://leetcode.cn/problems/combination-sum-iv/solutions/740581/zu-he-zong-he-iv-by-leetcode-solution-q8zv/)。

### 时空复杂度

> O(target*n),S(target)

### 源码

```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1);
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int& num : nums) {
            // 如果求组合数就是外层for循环遍历物品，内层for遍历背包。
			// 如果求排列数就是外层for遍历背包，内层for循环遍历物品。
                if (num <= i && dp[i - num] <= INT_MAX - dp[i]) {
                // num作为dp[i-num]的所有排列后的加num形成全新的排列
                // dp[i][j]=dp[i][j-num]+dp[i-1]
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
};

```

## 总结：

* <span style="color:red">关于完全背包问题，求排列和组合的区别见377.组合总和： 如果求组合数就是外层for循环遍历物品，内层for遍历背包； 如果求排列数就是外层for遍历背包，内层for循环遍历物品。特别需要注意求排列的思想，将num放在子排列的后面得到新的排列,dp[i]为所有的dp[i-num]之和，对于任意num属于nums</span>

