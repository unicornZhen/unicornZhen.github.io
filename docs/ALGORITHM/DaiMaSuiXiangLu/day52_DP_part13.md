# 300.最长递增子序列

[题目链接](https://leetcode.cn/problems/longest-increasing-subsequence/description/)

### 思路

> 动态规划。贪心和分治都不便于做这个题目，很容易想到动态规划解决，dp[i]为以nums[i]为起点的最大子序列的和,子问题为后i个元素的最长递增子序列的长度，动态转移方程见源代码，动态转移方程可以使用优先队列使得动态转移方程为O(logn)，关键想到是从后往前遍历。（dp[i]定义为以nums[i]为终点的最大子序列和的解题思想与上述类似）。

### 时空复杂度

> $$O(n^2),S(1)$$

### 源码

```C++
class Solution {
public:
    int lengthOfLIS(vector<int> &nums) {
        int len = (int) nums.size();
        vector<int> dp(len, 1);
        int result = 0;
        int LIS;
        for (int i = len - 1; i >= 0; i--) {
            LIS = 1;
            for (int j = i + 1; j < len; j++) {
                if (nums[j] > nums[i]) {
                    LIS = max(dp[j] + 1, LIS);
                }
            }
            dp[i] = LIS;
            result = max(result, LIS);
        }
        return result;
    }
};
```

# 674.最长连续递增序列

[题目链接](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/description/)

### 思路

> 应该属于贪心解法。题目比较简单，从左到右扫描一遍数组，每次贪心的得到连续的最长序列，所有的连续最长序列即为题目要找的最长连续递增序列。

### 时空复杂度

> O(n),S(1)

### 源码

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int> &nums) {
        int result = 1;
        int len = (int) nums.size();
        int start = 0;
        int end = 0;
        for (start = 0; start < len; start++) {
            end = start + 1;
            while (end < len && nums[end] > nums[end - 1]) {
                end++;
            }
            result = max(end - start, result);
            start = end - 1;
        }

        return result;
    }
};
```

# 718.最长重复子数组

[题目链接](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/description/)

### 思路

> 动态规划。解题思路来自300.最长递增子序列，dp\[i][j]表示以nums1[i]和nums2[j]结尾的最长公共子序列，时间和空间复杂度还是蛮高的，具体见源代码，关键难点是定义出dp的状态表，即找到合适的子问题。

### 时空复杂度

> $$O(n^2),S(n^2)$$

### 源码

```C++
class Solution {
public:
    int findLength(vector<int> &nums1, vector<int> &nums2) {
        int len1 = (int) nums1.size();
        int len2 = (int) nums2.size();
        int result = 0;
        // dp[i][j]表示以nums1[i]和nums2[j]结尾的最长公共子序列
        // dp[i][j]=dp[i-1][j-1]+1 or 0
        vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1, 0));
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (nums1[i-1] == nums2[j-1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    result = max(result, dp[i][j]);
                }
            }
        }
        return result;
    }
};
```

## 总结

* 子序列问题的dp表定义，有以nums[i]为结尾元素的题目要求的解，dp表的遍历顺序从头到尾;有以nums[i]为开头元素的题目要求的解，dp表的遍历顺序从尾到头。
