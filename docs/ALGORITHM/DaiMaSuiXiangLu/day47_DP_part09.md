# 198.打家劫舍

[题目链接](https://leetcode.cn/problems/house-robber/description/)

### 思路

> 动态规划。题目分析：状态应该是nums中的下标i,决策过程为是否打劫nums[i]，由于是否能打劫i与是否打劫i-1有关，故经过发散思维得到两种方法，其中一种方法参考[leetcode](https://leetcode.cn/problems/house-robber/)。
>
> 方法一：dp[i]以nums[i]为最后一家打劫对象的最大的抢劫金额的和，且dp[i]=max(dp[i-2],dp[i-3])+nums[i]。空间应该可以进一步优化，但优化将会导致代码编写麻烦，代码的结构也容易混乱，故不进行优化。
>
> 方法二：dp[i]为偷前k间房子的最大窃取金额，则dp[i]=max{dp[i-1],dp[i-2]+nums[i]}
>
> 第二种方法更好，思考的时候最先想到的是第二种方法，但以为第二种方法的转移方程不是最优解，实质上还是缺乏对转移方程的定义和数学归纳法的思考。

### 时空复杂度

> 设n为nums的长度
>
> O(n)，S(n)

### 源码

**方法一：**

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1){
            return nums[0];
        }
        if(nums.size()==2){
            return max(nums[0],nums[1]);
        }
        
        vector<int> dp(nums.size(),0);
        dp[0]=nums[0];
        dp[1]=nums[1];
        dp[2]=nums[2]+nums[0];
        for(int i=3;i<nums.size();i++){
            dp[i]=max(dp[i-2],dp[i-3])+nums[i];
        }
        return max(dp[nums.size()-1],dp[nums.size()-2]);
    }
};	
```

**方法二**

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1){
            return nums[0];
        }
        vector<int> dp(nums.size(),0);
        dp[0]=nums[0];
        dp[1]=max(nums[0],nums[1]);
        for(int i=2;i<nums.size();i++){
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[nums.size()-1];
    }
};
```



# 213.打家劫舍II

[题目链接](https://leetcode.cn/problems/house-robber-ii/description/)

### 思路

> 动态规划。改编自198.大家劫舍的方法二（即上一个题目），先求出打劫前n-1家的最大金额，再求出打劫后n-1家的最大金额，两者的最大值即得到答案。

### 时空复杂度

> O(n),S(n)(最好到达S(1))

### 源码

```C++
class Solution {
public:
    int rob(vector<int> &nums) {
        if (nums.size() == 1) {
            return nums[0];
        }
        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < nums.size() - 1; i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }

        int max_1 = dp[nums.size() - 2];
        dp[0] = 0;
        dp[1] = nums[1];
        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }

        return max(dp[nums.size() - 1], max_1);
    }
};
```

# 337.打家劫舍III

[题目链接](https://leetcode.cn/problems/house-robber-iii/description/)

### 思路

> 动态规划。跟968.监控二叉树有些类似，状态转移方程类似，但这个题目更加简单，首先这个应该是一个dp问题，子问题是其左右子树，由于相邻的节点不能被同时抢劫，故有三个状态需要不断转移，具体的状态说明和状态转移方程见源代码。

### 时空复杂度

> O(n)，S(h),其中h是树的高度，n是树的节点个数和。

### 源码

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    struct State {
        // 劫持根节点的最大抢劫金额
        int root{0};
        // 劫持整个left子树的最大抢劫金额
        int left{0};
        // 劫持整个right子树的最大抢劫金额
        int right{0};
    };

    int rob(TreeNode *root) {
        State result = dfs(root);
        return max(result.root, result.left + result.right);
    }

private:
    State dfs(TreeNode *root) {
        if (root == nullptr) {
            return {0, 0, 0};
        }
        State state_l = dfs(root->left);
        State state_r = dfs(root->right);
        State max_root;
        max_root.left = max(state_l.root, state_l.left + state_l.right);
        max_root.right = max(state_r.root, state_r.right + state_r.left);
        max_root.root = max(state_l.left + state_l.right + state_r.left + state_r.right + root->val,
                            state_l.root + state_r.root);
        return max_root;
    }
};
```

## 总结

* 337.打家劫舍III与968.监控二叉树关于状态的设计十分类似

  