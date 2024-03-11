# 1143.最长公共子序列	

[题目链接](https://leetcode.cn/problems/longest-common-subsequence/description/)

### 思路

> 动态规划。与718.最长重复子数组解题思想类似，没有想到状态转移方程为什么那样是合理的，合理性说明：dp\[i][j]分为两部分，以text1[i]和text2[j]结尾的最长公共子序列和不以它为结尾的最长公共子序列,分别对应dp\[i][j] = dp\[i - 1][j - 1] + 1和max(dp\[i - 1][j], dp\[i][j - 1])。

### 时空复杂度

> $$O(n^2),S(n^2)$$

### 源码

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int len1 = (int) text1.length();
        int len2 = (int) text2.length();
        // dp[i][j]表示text1前i个字符和text2前j个字符的最长公共子序列
        // dp[i][j]的状态转移分为以text1[i]和text2[j]结尾的最长公共子序列和不以它为结尾的最长公共子序列
        // 可以使用滚动数组将空间复杂度优化至O(n)
        vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1, 0));
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[len1][len2];
    }
};
```

# 1035.不相交的线

[题目链接](https://leetcode.cn/problems/uncrossed-lines/description/)

### 思路

> 动态规划。与1143.最长公共子序列题目一样，即前一个题目。本质上是求nums1和nums2的最长公共子序列s1和s2，s1和s2中每个对应元素连线即满足题意，故转化为1143.最长公共子序列。

### 时空复杂度

> $$O(n^2)，S(n^2)$$

### 源码

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int> &nums1, vector<int> &nums2) {
        int len1 = (int) nums1.size();
        int len2 = (int) nums2.size();
        // dp[i][j]表示nums1前i个元素和nums2前j个元素的最大不相交的线和
        vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1, 0));

        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (nums1[i-1] == nums2[j-1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[len1][len2];
    }
};
```

# 53.最大子序和

[题目链接](https://leetcode.cn/problems/maximum-subarray/description/)

### 思路

> 动态规划，也可以说是贪心思想，具体就不用纠结了。比较简单，具体见源代码。

### 时空复杂度

> O(n),S(n)(可以进一步优化至S(1))

### 源码

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        int result = dp[0];
        for (int i = 1; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1] + nums[i], nums[i]); // 状态转移公式
            if (dp[i] > result) result = dp[i]; // result 保存dp[i]的最大值
        }
        return result;
    }
};
```

