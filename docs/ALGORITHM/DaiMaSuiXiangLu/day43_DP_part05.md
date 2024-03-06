# 1049.最后一块石头的重量

[题目链接](https://leetcode.cn/problems/last-stone-weight-ii/description/)

### 思路

> 动态规划。属于0-1背包的变种题目，首先将stones分为两个和最接近的子数组nums1和nums2,石头的粉碎过程的结果可以用一系列的加减表示，这一系列的加减结果不会小于fabs(sum(nums1)-sum(nums2))，而如果划分出nums1和nums2,则nums1和nums2本身给出了一个石头的粉碎顺序，故综上所述题目求的是将stones划分成和最接近的子数组的和。

### 时空复杂度

> O(n*sum),S(sum)

### 源码

```
class Solution {
public:
    int lastStoneWeightII(vector<int> &stones) {
        int sum = 0;
        for (auto it: stones) {
            sum += it;
        }
        int bag_size = sum / 2;
        vector<int> dp(bag_size + 1,0);

        for(auto stone:stones){
            for(int i=bag_size;i>=stone;i--){
                dp[i]=max(dp[i],dp[i-stone]+stone);
            }
        }

        return abs(sum-dp[bag_size]*2);
    }
};
```

# 494.目标和

[题目链接](https://leetcode.cn/problems/target-sum/description/)

### 思路

> 动态规划。首先这是一个动态规划的问题，识别过程省略。状态（i,j）表示nums前i个元素目标和为j的合法数量，转移方程见源代码，注意j的范围为[-sum,sum]，其中sum为nums的绝对值之和。
>
> 感觉会有一些的降低时间复杂度的技巧，降低时间复杂度的方法见[代码随想录题解](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html#%E6%80%9D%E8%B7%AF)，通过求装满容量为(sum-target)/2的背包的装法数量。

### 时空复杂度

> 设sum为nums中所有元素绝对值的和，n为nums的大小，则时间复杂度为O(n*sum),S(1)

### 源码

```
class Solution {
public:
    int findTargetSumWays(vector<int> &nums, int target) {
        int sum = 0;
        for (auto it: nums) {
            sum += abs(it);
        }
        vector<int> pre_dp(2 * sum + 1, 0);
        vector<int> cur_dp(2 * sum + 1, 0);
        pre_dp[sum] = 1;
        for (auto num: nums) {
            for (int i = 0; i < 2 * sum + 1; i++) {
                cur_dp[i] = 0;
                if (i - num >= 0) {
                    cur_dp[i] += pre_dp[i - num];
                }
                if (i + num < 2 * sum + 1) {
                    cur_dp[i] += pre_dp[i + num];
                }
            }
            pre_dp = cur_dp;
        }
        if(target>sum||target<-sum){
            return 0;
        }
        return cur_dp[target + sum];
    }
};
```

# 474.一和零

[题目链接](https://leetcode.cn/problems/ones-and-zeroes/description/)

### 思路

> 动态规划。本质上是一个三维背包问题，所以没有可行的贪心解法。状态转移方程见源代码，在此省略。

### 时空复杂度

> 设l为strs的长度，L为strs所有字符串的长度和，m和n分别为给定的两个整数,则时间复杂度为O(lmn+L),空间复杂度为S(mn)

### 源码

```
class Solution {
public:
    int findMaxForm(vector<string> &strs, int m, int n) {
        vector<int> count_one(strs.size(), 0);
        for (int i = 0; i < strs.size(); i++) {
            for (auto it: strs[i]) {
                if (it == '1') {
                    count_one[i]++;
                }
            }
        }
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int k = 0; k < strs.size(); k++) {
            int count_zero = (int) strs[k].size() - count_one[k];
            // dp[i][j][k]=max{dp[i-1][j-count_one][k-count_zero]+1,dp[i-1][j][k]
            for (int i = m; i >= count_zero; i--) {
                for (int j = n; j >= count_one[k]; j--) {
                    dp[i][j] = max(dp[i - count_zero][j - count_one[k]] + 1, dp[i][j]);
                }
            }
        }
        return dp[m][n];
    }
};
```

## 总结

* 两个题目属于背包这一类的动态规划问题，识别题目应该采用什么方法做很重要。
