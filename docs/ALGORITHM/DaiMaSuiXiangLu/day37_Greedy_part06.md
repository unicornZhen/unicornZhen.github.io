# 738.单调递增的数字

[题目链接](https://leetcode.cn/problems/monotone-increasing-digits/description/)

### 思路

> 找到数字num最小的需要减1的数位的位置flag，后面的数位都可以置为9，属于贪心法的思想吧，但又好像关系不
>
> 大

### 时空复杂度

> $$O(n),S(1)$$

### 源码

```C++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string str = to_string(n);
        int m = str.length();
        int flag = m;
        for (int i = m - 1; i > 0; --i) {
            if (str[i - 1] > str[i]) {
                flag = i;
                str[i - 1]--;
            }
        }
        for (int i = flag; i < m; ++i) {
            str[i] = '9';
        }
        return stoi(str);
    }
};
```

# 968.监控二叉树

[题目链接](https://leetcode.cn/problems/binary-tree-cameras/description/)

### 思路

> 贪心思想。[代码随想录](https://programmercarl.com/0968.%E7%9B%91%E6%8E%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF)，贪心思想解决问题的初步探索，进而得出[leetcode题解](https://leetcode.cn/problems/binary-tree-cameras/solutions/422860/jian-kong-er-cha-shu-by-leetcode-solution/)的状态转移方程的支撑，

### 时空复杂度

> O(n),S(n)

### 源码

```C++
struct Status {
// 状态a：root 必须放置摄像头的情况下，覆盖整棵树需要的摄像头数目。
// 状态b：覆盖整棵树需要的摄像头数目，无论 root 是否放置摄像头。
// 状态c：覆盖两棵子树需要的摄像头数目，无论节点 root 本身是否被监控到
    int a, b, c;
};

class Solution {
public:
    Status dfs(TreeNode* root) {
        if (!root) {
            return {INT_MAX / 2, 0, 0};
        }
        auto [la, lb, lc] = dfs(root->left);
        auto [ra, rb, rc] = dfs(root->right);
        int a = lc + rc + 1;
        int b = min(a, min(la + rb, ra + lb));
        int c = min(a, lb + rb);
        return {a, b, c};
    }

    int minCameraCover(TreeNode* root) {
        auto [a, b, c] = dfs(root);
        return b;
    }
};
```

## 总结

* 968.监控二叉树的贪心思想设计而出来的状态转移很值得学习，目前还不能完全看懂，这个留待后面有时间就看
* 贪心思想很多来自于日常生活常识，这个很重要
* 分发糖果对于两次分发十分新颖，两个维度分别考虑再和在一起处理有时候会更加简单
