# 1005.k次取反后最大化的数组和

[题目链接](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/description/)

### 思路

> 贪心思想。考虑对nums数组的k次取反，优先将最小的负数取反，即每次取反时最大化增加数组和。

### 时空复杂度

> 需要先对nums进行排序，为了简化取反时的判断与nums全部值大于0但k大于0,故选择使用绝对值的比较作为排序的比较函数。故时间复杂度$$O(nlogn),S(logn)$$（空间复杂度取决于排序的空间复杂度，sort一般采用快排为O(nlogn)）。

### 源码

```C++
class Solution {
static bool cmp(int a, int b) {
    return abs(a) > abs(b); 
}
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), cmp);
        for (int i = 0; i <nums.size(); i++) {
            if(nums[i] < 0 && k > 0) {
                nums[i] *= -1;
                k--;
            }
        }

        if (k % 2 == 1) nums[nums.size() - 1] *= -1;
        int result = 0;
        for(int a:nums) result += a;
        return result;
    }
};
```

# 134.加油站

[题目链接](https://leetcode.cn/problems/gas-station/description/)

### 思路

> 两种方法，实质上证明思想类似，都比较巧妙，借助了反证法的思想。具体都是通过计算gas与cost的差数组，解题思想参考于[代码随想录](https://programmercarl.com/0134.%E5%8A%A0%E6%B2%B9%E7%AB%99.html#%E6%80%9D%E8%B7%AF)。leetcode的官方答案题解就是代码随想录的解法二。

### 时空复杂度

> O(n),S(1)

### 源码（下面仅展示方法一的解答）

```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int min = INT_MAX; // 从起点出发，油箱里的油量最小值
        for (int i = 0; i < gas.size(); i++) {
            int rest = gas[i] - cost[i];
            curSum += rest;
            if (curSum < min) {
                min = curSum;
            }
        }
        if (curSum < 0) return -1;  // 情况1
        if (min >= 0) return 0;     // 情况2
                                    // 情况3
        for (int i = gas.size() - 1; i >= 0; i--) {
            int rest = gas[i] - cost[i];
            min += rest;
            if (min >= 0) {
                return i;
            }
        }
        return -1;
    }
};
```

# 135.分发糖果

[题目链接](https://leetcode.cn/problems/candy/description/)

### 思路

> 总体来说都是应用贪心思想，优先分配最少的糖果。
>
> 方法一：两遍扫描。将相邻的孩子中，评分高的孩子必须获得更多的糖果拆分为左规则和右规则，最后取左规则和右规则的各个位置的最大值即所要分配的糖果，思想十分巧妙，关键在于规则的划分。
>
> 方法二：一遍扫描。不断确定极值点（即找单调区间），注意ratings中相邻两个值相等的情况，但自己想的较为代码较为繁琐，借鉴题目的代码。

### 时空复杂度

> 时间复杂度都是O(n)
>
> 空间复杂度第一种方法是O(n)，第二种方法是O(1)。

### 源码

**方法一源码**

```C++
class Solution {
// TODO：可以进一步优化空间，去掉r_candies，只扫描两遍。
public:
    int candy(vector<int> &ratings) {
        int result = 0;
        vector<int> l_candies(ratings.size(), 1);
        vector<int> r_candies(ratings.size(), 1);

        // 将满足的规则拆分为左规则和右规则
        for (int i = 1; i < ratings.size(); i++) {
            if (ratings[i] > ratings[i - 1]) {
                l_candies[i] = l_candies[i - 1] + 1;
            }
        }
        for (int i = ratings.size() - 1; i >= 1; i--) {
            if (ratings[i - 1] > ratings[i]) {
                r_candies[i - 1] = r_candies[i] + 1;
            }
        }

        for (int i = 0; i < ratings.size(); i++) {
            result += (l_candies[i] > r_candies[i] ? l_candies[i] : r_candies[i]);
        }
        return result;
    }
};
```

**方法二源码**

```                                              C++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        int ret = 1;
        //dec用的很巧妙，dec可以计算降序对于糖果分配的影响，即替代了方法一中的r_candies
        //dec极大的减少了代码逻辑的混乱，相比自己个人的思考
        int inc = 1, dec = 0, pre = 1;
        for (int i = 1; i < n; i++) {
            if (ratings[i] >= ratings[i - 1]) {
                dec = 0;
                pre = ratings[i] == ratings[i - 1] ? 1 : pre + 1;
                ret += pre;
                inc = pre;
            } else {
                dec++;
                if (dec == inc) {
                    dec++;
                }
                ret += dec;
                pre = 1;
            }
        }
        return ret;
    }
};
```

## 总结

* K次最大取反数组和抓住了贪心策略的本质，使用绝对值的比较作为比较函数，精简代码
* 加油站的贪心思想比较巧妙，值的多推敲多思考，不论是方法一还是方法二
* 分发糖果规则的拆分，以及设计dec精简代码，与上述的第一点相同