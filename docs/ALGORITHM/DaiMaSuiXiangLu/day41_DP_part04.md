# 416.分割等和子集

[题目链接](https://leetcode.cn/problems/partition-equal-subset-sum/description/)

### 思路

> 动态规划。此问题可以转化为背包问题，设sum为数组nums所有元素的和，此问题转为背包容量为sum/2，nums描述物品重量，判断背包的能装下的最大值是否等于背包的容量。在这个过程中可以进一步优化，dp\[i][j]表示背包容量为j是否能够被前i件物品恰好装满;空间优化，滚动数组，从后往前遍历。

### 时空复杂度

> 设n为nums的大小，sum为nums所有元素的和
>
> O(n*sum),S(sum)

### 源码

```
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for (auto num : nums) {
            sum += num;
        }
        if (sum % 2 != 0) {
            return false;
        }
        int target = sum / 2;
        vector<bool> dp(target + 1, false);
        dp[0] = true;
        for (auto num : nums) {
            for (int i = target; i >= num; i--) {
                dp[i] = dp[i] || dp[i - num];
            }
        }
        return dp[target];
    }
};
```

