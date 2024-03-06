# 435.无重叠区间

[题目链接](https://leetcode.cn/problems/non-overlapping-intervals/description/)

### 思路

> 贪心思想。贪心解法十分巧妙，首先求出尽可能多的重叠区间，这个通过对区间的右端点排序解决。然后尽可能是的保留的所有区间靠近左侧，对于区间的左端点排序与此类似，解题思想参考自[代码随想录](https://programmercarl.com/0435.%E6%97%A0%E9%87%8D%E5%8F%A0%E5%8C%BA%E9%97%B4.html)。

### 时空复杂度

> O(n),S(1)

### 源码

```
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.empty()) {
            return 0;
        }
        
        sort(intervals.begin(), intervals.end(), [](const auto& u, const auto& v) {
            return u[1] < v[1];
        });

        int n = intervals.size();
        int right = intervals[0][1];
        int ans = 1;
        for (int i = 1; i < n; ++i) {
            if (intervals[i][0] >= right) {
                ++ans;
                right = intervals[i][1];
            }
        }
        return n - ans;
    }
};
```

# 763.划分字母区间

[题目链接](https://leetcode.cn/problems/partition-labels/description/)

### 思路

> 贪心思想。容易想到，具体见代码。

### 时空复杂度

> O(n),S(1)

### 源码

```

class Solution {
public:
    vector<int> partitionLabels(string s) {
        vector<int> result;
        vector<int> show_pos(26, 0);
        for (int i = 0; i < s.size(); i++) {
            if (i > show_pos[s[i] - 'a']) {
                show_pos[s[i] - 'a'] = i;
            }
        }
        int min_end = show_pos[s[0] - 'a'];
        int start = 0;
        for (int i = 1; i < s.size(); i++) {
            if (i > min_end) {
                result.push_back(i - start);
                min_end = show_pos[s[i]-'a'];
                start = i;
            } else {
                min_end = min_end > show_pos[s[i] - 'a'] ? min_end : show_pos[s[i] - 'a'];
            }
        }
        result.push_back(min_end - start + 1);
        return result;
    }
};
```

# 56.合并区间

[题目链接https://leetcode.cn/problems/merge-intervals/description/]()

### 思路

> 贪心法。与435类似，具体思路见源代码。

### 时空复杂度

> O(n),S(1)

### 源码

```
class Solution {
public:
    static bool cmp(const vector<int> &a, const vector<int> &b) {
        return a[0] < b[0];
    }

    vector<vector<int>> merge(vector<vector<int>> &intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        vector<vector<int>> result;
        int max_end = intervals[0][1];
        int start = intervals[0][0];
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] > max_end) {
                result.push_back({start, max_end});
                start = intervals[i][0];
                max_end = intervals[i][1];
            } else {
                max_end = max_end > intervals[i][1] ? max_end : intervals[i][1];
            }
        }
        result.push_back({start,max_end});
        return result;
    }
};

```

## 总结

* 以上的题目基本都属于重叠区间的题目，这类题目的解题思想来自于日常的射气球，即[452.用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/description/)，都是先通过排序是的重叠的区间尽可能在一起，再按照贪心思想做决策。
