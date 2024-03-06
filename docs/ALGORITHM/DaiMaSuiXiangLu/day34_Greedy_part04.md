# 860.柠檬水找零

[题目链接](https://leetcode.cn/problems/lemonade-change/description/)

### 思路

> 贪心。找零时使用贪心，日常生活中的找零钱问题，很简单，具体见代码。

### 时空复杂度

> 时间复杂度：O(n)
>
> 空间复杂度：由于只有5,10,20三种类型的钱，故是S(1)

### 源码

```
class Solution {
public:
    bool lemonadeChange(vector<int> &bills) {
        int changes_5 = 0;
        int changes_10 = 0;
        for (auto it: bills) {
            switch (it) {
                case 5:
                    changes_5++;
                    break;
                case 10:
                    changes_10++;
                    changes_5--;
                    if (changes_5 < 0) {
                        return false;
                    }
                    break;
                case 20:
                    if (changes_10 >= 1) {
                        changes_10--;
                        changes_5 -= 1;
                    } else {
                        changes_5 -= 3;
                    }
                    if (changes_5 < 0) {
                        return false;
                    }
                    break;
                default:
                    return false;
            }
        }
        return true;
    }
};
```

# 406.根据身高重建队列

[题目链接](https://leetcode.cn/problems/queue-reconstruction-by-height/description/)

### 思路

> 日常的排队处理，在此我看不到贪心的思想，暂且就这样做吧。
>
> 看到这样两个维度people[i]=[hi,ki]，首先对身高hi排序得到一个从低到高的序列，再将这个序列依照其中对应的ki依次确定每个人的位置，这种确定过程是唯一确定的。

### 时空复杂度

> $$O(n^2),S(1)$$

### 源码

```
class Solution {
public:
    static bool cmp(const vector<int> &a, const vector<int> &b) {
        return a[0] < b[0];
    }

    vector<vector<int>> reconstructQueue(vector<vector<int>> &people) {
        vector<vector<int>> result(people.size(), {-1, -1});
        sort(people.begin(), people.end(),cmp);
        int count = 0;
        for (auto &it: people) {
            count = 0;
            for (auto &it1: result) {
                if (count >= it[1]) {
                    if(it1[0]>=0){
                        continue;
                    }
                    it1[0] = it[0];
                    it1[1] = it[1];
                    break;
                }
                if (it1[0] == -1 || it1[0] == it[0]) {
                    count++;
                }
            }
        }
        return result;
    }
};
```

# 452.用最少数量的箭引爆气球

[题目链接](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/description/)

### 思路

> 与part05重叠区间类似，在此不赘述。值得注意的是射出的气球坐标是在某个气球区间的最右侧，贪心的证明与part05的类似，不赘述。

### 时空复杂度

> $$O(nlogn),S(1)$$

### 源码

```
class Solution {
public:
    static bool cmp(const vector<int> &a, const vector<int> &b) {
        return a[0] < b[0];
    }

    int findMinArrowShots(vector<vector<int>> &points) {
        int result = 0;
        sort(points.begin(), points.end(), cmp);
        int overlap_end = points[0][1];
        for (auto it: points) {
            if (it[0] > overlap_end) {
                result++;
                overlap_end = it[1];
            } else {
            	// 注意更新overlap_end
                overlap_end = min(overlap_end, it[1]);
            }
        }
        // 注意对最后一个气球区间的射爆
        return result + 1;
    }
};
```

