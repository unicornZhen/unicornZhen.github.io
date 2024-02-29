# 455.分发饼干

[题目链接](https://leetcode.cn/problems/assign-cookies/description/)

### 思路

> 贪心思想。按照日常生活知识推断优先满足具有最小贪心因子的小孩可以使得满意的小孩数最大，这个可以通过反证法证明得到。然后考虑使用双指针和排序优化算法。

### 时空复杂度

> 设n为g的长度，m为s的长度
>
> O($$nlogn+mlogm$$),S(1)

### 源码

```
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());

        int result=0;
        int i_g=0,i_s=0;
        while(i_g<g.size()&&i_s<s.size()){
            if(s[i_s]>=g[i_g]){
                i_g++;
                i_s++;
                result++;
            }else{
                i_s++;
            }
        }
        return result;
    }
};
```

# 376.摆动序列

[题目链接](https://leetcode.cn/problems/wiggle-subsequence/description/)

### 思路

> 贪心思想。
>
> 设最长摆动序列为target。贪心的策略分为两步走：i.选择nums[0]作为target[0]的第一个元素；ii.target的其余的target[i]都是都是nums在target[i-1]与target[i]中的第一个极值点。经过数学上的证明可以得到这样的贪心策略是能得到最长摆动序列。

### 时空复杂度

> O(n),S(1)

### 源码

```
class Solution {
public:
    int wiggleMaxLength(vector<int> &nums) {
        if (nums.size() <= 1) {
            return 1;
        }
        int result = 1;
        int i = 1;
        int isnegative = -1;
        while (i < nums.size() && nums[i] == nums[0]) {
            i++;
        }
        if (i >= nums.size()) {
            return 1;
        }
        if (nums[i] > nums[0]) {
            isnegative = 1;
        }
        for (; i < nums.size(); i++) {
            if (nums[i] * isnegative < nums[i - 1] * isnegative) {
                isnegative *= -1;
                result++;
            }
        }
        return result + 1;
    }
};
```

# 53.最大子数组和

[题目链接](https://leetcode.cn/problems/maximum-subarray/)

### 思路

> 动态规划，之前做过现在有些忘了，直觉感觉是使用dp方法，并不属于贪心吧。需要使用dp、dp1两个表，其中dp[i]是指nums[0..i]之间的最大子数组和，dp1[i]指的是以nums[i]结尾的最大子数组和。由于使用markdown不便于编写dp的转移方程，具体见下面的源码。

### 时空复杂度

> O(n),S(1)

### 源码

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        long long sum=nums[0];
        long long max_last_sum=nums[0];
        for(int i=1;i<nums.size();i++){
            max_last_sum=max_last_sum<0?nums[i]:max_last_sum+nums[i];
            sum=(sum>max_last_sum?sum:max_last_sum);
        }
        return sum;
    }
};
```

## 总结

* 识别贪心问题和动态规划问题，然后尝试使用转移方程或者贪心策略解决几个实际测例，最后证明并写出代码

  
