# 122.买卖股票的最佳时机II

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

### 思路

> 方法一：动态规划。动态规划具有通用性，可以用来解决一系列这样的问题。
>
> 方法二：贪心。本题由于一定的特殊性，可以使用贪心来解决问题，通过差分的概念可以证明贪心策略是正确的。
>
> 本题采用方法二解答。

### 时空复杂度

> O(n),S(1)

### 源码

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result=0;
        for(int i=1;i<prices.size();i++){
            if(prices[i]>prices[i-1]){
                result+=(prices[i]-prices[i-1]);
            }
        }
        return result;
    }
};
```

# 55.跳跃游戏

[题目链接](https://leetcode.cn/problems/jump-game/description/)

### 思路

> 贪心法。按照日常知识选择贪心策略，决策过程是选择不断跳到最好的新的起跳点，即nums[i]+i是所有可以到达的位置中的最大，可以解决问题，但可以进一步优化时间复杂度到O(n)

### 时空复杂度

> 自己写的代码：最坏时间复杂度可以达到O($$n^2$$)
>
> leetcode官方题解进一步改进，时间复杂度稳定为O(n),空间复杂度两者都是S(1)

### 源码

```C++
//自己的代码
class Solution {
public:
    bool canJump(vector<int> &nums) {
        int i=0;
        int end=0;
        while(i<nums.size()){
            if (nums[i] + i >= nums.size()-1) {
                return true;
            }
            if (nums[i] <= 0) {
                return false;
            }
            // to find the next position by greedy way
            end = i + 1;
            for (int j = 2; j <= nums[i]; j++) {
                if (nums[i + j] + i + j >= nums[end] + end) {
                    end = i + j;
                }
            }
            i = end;
        }
        return false;
    }
};


//官方代码
public class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int rightmost = 0;
        for (int i = 0; i < n; ++i) {
            if (i <= rightmost) {
                rightmost = Math.max(rightmost, i + nums[i]);
                if (rightmost >= n - 1) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

# 45.跳跃游戏II

[题目链接](https://leetcode.cn/problems/jump-game-ii/description/)

### 思路

> 贪心思想。与55.跳跃游戏解法与改进类似，在此不再赘述

### 时空复杂度

> O(n),S(1)

### 源码

```C++
class Solution {
    public int jump(int[] nums) {
        int length = nums.length;
        int end = 0;
        int maxPosition = 0; 
        int steps = 0;
        for (int i = 0; i < length - 1; i++) {
            maxPosition = Math.max(maxPosition, i + nums[i]); 
            if (i == end) {
                end = maxPosition;
                steps++;
            }
        }
        return steps;
    }
}
```

