# 509.斐波那契数

[题目链接](https://leetcode.cn/problems/fibonacci-number/description/)

### 思路

> DP法。典型且很简单，在此不赘述。

### 时空复杂度

> n为F(n)中的n
>
> O(n),S(1)

### 源码

```C++
class Solution {
public:
    int fib(int n) {
        if(n<=0){
            return 0;
        }
        if(n==1){
            return 1;
        }
        int n_1=1;
        int n_2=0;
        int result;
        for(int i=2;i<=n;i++){
            result=n_1+n_2;
            n_2=n_1;
            n_1=result;
        }
        return result;
    }
};
```

# 70.爬楼梯

[题目链接](https://leetcode.cn/problems/climbing-stairs/description/)

### 思路

> DP法。状态转移方程和状态与斐波那契数列一样，唯一有区别的是边界值，f(0)=1.

### 时空复杂度

> O(n),S(1)

### 源码

```C++
class Solution {
public:
    int climbStairs(int n) {
    // 注意这里与斐波那契数不一样，在斐波那契数中f(0)=0
        int cur_2=1;
        int cur_1=1;
        int result=1;
        for(int i=2;i<=n;i++){
            result=cur_1+cur_2;
            cur_2=cur_1;
            cur_1=result;
        }
        return result;
    }
};
```

# 746.使用最小花费爬楼梯

[题目链接](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)

### 思路

> DP法。符合DP问题的特点，决策模型、最优子结构、重叠问题等，最初想使用贪心解决这个问题，仔细思考是行不通的，因为每次只能跳一个或者两个台阶，按照此种决策模型是不行的。状态转移很简单具体见代码，在此不赘述。

### 时空复杂度

> O(n)，S(1)

### 源码

```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int cur_2=0;
        int cur_1=0;
        int cur;
        for(int i=2;i<=cost.size();i++){
            cur=min(cur_2+cost[i-2],cur_1+cost[i-1]);
            cur_2=cur_1;
            cur_1=cur;
        }
        return cur;
    }
};
```

## 总结

* 动态规划解题步骤：

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

* 入门dp问题的解决