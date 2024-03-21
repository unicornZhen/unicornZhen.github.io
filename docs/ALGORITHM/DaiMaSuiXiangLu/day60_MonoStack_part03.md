## 84.柱状图中最大的矩形

[84. 柱状图中最大的矩形 - 力扣（LeetCode）](https://leetcode.cn/problems/largest-rectangle-in-histogram/description/)

### 思路

> 单调栈。首先从蛮力法入手思考，注意到最大矩形的高度一定是heights中的一个值，对于蛮力法来说，我们可以遍历所有可能的高度或宽度来求得最大的矩形，这时间复杂度为O(n),而n最大为$$10^5$$，这样肯定超时，故考虑优化高度或者宽度的蛮力求解法。注意到可以用单调栈优化高度的蛮力法，故可以得到最终所需要的求解方法。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    int largestRectangleArea(vector<int> &heights) {
        int len = (int) heights.size();
        int result = 0;
        // 最后一个不低于i的最左边的柱子的索引
        vector<int> l_smaller(len);
        // 最后一个不低于i的最右边的柱子的索引
        vector<int> r_smaller(len);
        for (int i = 0; i < len; i++) {
            l_smaller[i] = 0;
            r_smaller[i] = len-1;
        }
        stack<int> s_index;
        for (int i = 0; i < len; i++) {
            while (!s_index.empty() && heights[s_index.top()] > heights[i]) {
                r_smaller[s_index.top()] = i - 1;
                s_index.pop();
            }
            s_index.push(i);
        }
        while (!s_index.empty()) {
            s_index.pop();
        }
        for (int i = len - 1; i >= 0; i--) {
            while (!s_index.empty() && heights[s_index.top()] > heights[i]) {
                l_smaller[s_index.top()] = i + 1;
                s_index.pop();
            }
            s_index.push(i);
        }
        for (int i = 0; i < len; i++) {
            result = max(result, heights[i] * (r_smaller[i] - l_smaller[i] + 1));
        }
        return result;
    }
};
```

## 总结

* 单调栈和单调队列的用法要记住
* 代码随想录一刷完结，还是觉得对于自己有较大的提升，虽然报那个班属实是浪费钱，跟着代码随想录公开的题目刷就可以了
