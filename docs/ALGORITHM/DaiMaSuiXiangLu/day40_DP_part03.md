# 343.整数拆分

[题目链接](https://leetcode.cn/problems/integer-break/description/)

### 思路

> 方法一：DP。DP法时间复杂度是O($$n^2$$)在此不赘述，状态是i,dp[i]表示数字i的最大拆分乘积，具体见[leetcode](https://leetcode.cn/problems/integer-break/solutions/352875/zheng-shu-chai-fen-by-leetcode-solution/)
>
> 方法二：贪心法。需要确定贪心的策略，两点：什么样的数字应该拆分（大于或者等于4应该拆分）；应该拆分成什么数字（拆分成3最好）

### 时空复杂度

> 计算$$pow(3,n/3)$$至少需要logn的时间,故时间复杂度至少为O(logn),空间复杂度为S(1)

### 源码

```
class Solution {
public:
    int integerBreak(int n) {
        if (n <= 3) {
            return n - 1;
        }
        int quotient = n / 3;
        int remainder = n % 3;
        if (remainder == 0) {
            return (int)pow(3, quotient);
        } else if (remainder == 1) {
            return (int)pow(3, quotient - 1) * 4;
        } else {
            return (int)pow(3, quotient) * 2;
        }
    }
};
```

# 96.不同的二叉搜索树

[题目链接](https://leetcode.cn/problems/unique-binary-search-trees/description/)

### 思路

> 动态规划。经过尝试构造BST和思考，构造大规模的BST可以基于构造小规模的BST完成，这属于典型的动态规划题目，具体见源代码，可以先取一个节点作为树根，树根的左右子树的BST数量的乘积即为以这个节点为跟节点的不同BST树的数量和。

### 时空复杂度

> O($$n^2$$),S(n)

### 源码

```
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n+1,0);
        dp[0]=1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=i;j++){
                dp[i]+=dp[i-j]*dp[j-1];
            }
        }
        return dp[n];
    }
};
```

