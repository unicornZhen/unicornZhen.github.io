## 739.每日温度

[739. 每日温度 - 力扣（LeetCode）](https://leetcode.cn/problems/daily-temperatures/description/)

### 思路

> 方法一：暴力法。注意到温度的范围为0到100，故我们设置一个数组show[0:101],初始时值为无限大，show[i]表示温度i出现的最早时间，从后往前遍历temperatures，遍历时更新show数组同时得到第i天的答案。
>
> 方法二：单调栈。经分析题目，发现备选答案之间有遮挡的关系，符合单调栈的应用特征，具体可以见源代码。
>
> 本题采用单调栈的做法。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int> &temperatures) {
        stack<int> index;
        int len = (int) temperatures.size();
        vector<int> result(len, 0);
        for (int i = 0; i < len; i++) {
            while (!index.empty() && temperatures[index.top()] < temperatures[i]) {
                result[index.top()] = i - index.top();
                index.pop();
            }
            index.push(i);
        }
        return result;
    }
};
```

## 496.下一个更大的元素

[496. 下一个更大元素 I - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-i/description/)

### 思路

> 基于739.每日温度的方法做，也包括暴力法和单调栈法，本题采用单调栈法，具体见源代码。

### 时空复杂度

> m为nums1的长度，n为nums2的长度。
>
> 哈希表的查询复杂度为O(1)（况且nums1每个元素还不同，故时间复杂度更加低）：O(m+n)
>
> S(n)

### 源码

```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int> &nums1, vector<int> &nums2) {
        stack<int> index;
        int len2 = (int) nums2.size();
        int len1 = (int) nums1.size();
        unordered_map<int, int> m;
        vector<int> result(len1, -1);
        for (int i = 0; i < len1; i++) {
            m[nums1[i]] = i;
        }
        for (int i = 0; i < len2; i++) {
            while (!index.empty() && nums2[index.top()] < nums2[i]) {
                if (m.find(nums2[index.top()]) != m.end()) {
                    result[m.at(nums2[index.top()])] = nums2[i];
                }
                index.pop();
            }
            index.push(i);
        }
        return result;
    }
};
```

## 总结

* 如何识别出使用单调栈、单调队列解决问题很重要，主要时备选答案遮挡的思想。