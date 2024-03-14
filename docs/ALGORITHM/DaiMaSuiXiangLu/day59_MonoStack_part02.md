## 503.下一个更大的元素II

[503. 下一个更大元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-ii/description/)

### 思路

> 单调栈。与496.下一个更大的元素基本一致，由于是一个循环数组，故我们选择遍历数组两次，这样就能够满足寻找循环数组nums中的每一个元素的下一个的最大值。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int len=(int)nums.size();
        vector<int> result(len,-1);
        stack<int> index;
        for(int k=1;k<=2;k++) {
            for (int i = 0; i < len; i++) {
                while (!index.empty() && nums[index.top()] < nums[i]) {
                    result[index.top()] = nums[i];
                    index.pop();
                }
                index.push(i);
            }
        }
        return result;
    }
};
```

## 42.接雨水

[42. 接雨水 - 力扣（LeetCode）](https://leetcode.cn/problems/trapping-rain-water/description/)

### 思路

> 最开始想的是找到每个连续的水洼的开始柱子和结束柱子，后面发现这个无法解决从全局的水洼即更大的水洼。故换一种思路，计算每个柱子可以承接的接水量，累加起来就是所求结果。如何计算每个柱子可以承接的水量呢，通过计算把这个柱子圈起来的整个水洼的最左边和最右边的柱子，即左边最大的柱子和右边最大的柱子，这个可以通过动态规划得到，时间复杂度为O(n)，可行。这个想法可以进一步优化为双指针法，源代码有些难以理解。
>
> 还有可以通过单调栈做，在这里由于时间关系，在此不赘述。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    int trap(vector<int> &height) {
        //求两个数组，左边最大的柱子和右边最大的柱子
        int len = (int) height.size();
        vector<int> l_max = height;
        vector<int> r_max = height;
        int result = 0;
        int max_h = height[0];
        for (int i = 1; i < len; i++) {
            max_h = max(height[i], max_h);
            l_max[i] = max_h;
        }

        max_h = height[len - 1];
        for (int i = len - 2; i >= 0; i++) {
            max_h = max(height[i], max_h);
            r_max[i] = max_h;
        }
        for (int i = 0; i < len; i++) {
            result += min(l_max[i], r_max[i]) - height[i];
        }
        return result;
    }
};
```

**进一步使用双指针优化空间复杂度**

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        int left = 0, right = height.size() - 1;
        int leftMax = 0, rightMax = 0;
        while (left < right) {
            leftMax = max(leftMax, height[left]);
            rightMax = max(rightMax, height[right]);
            if (leftMax < righ) {
                ans += leftMax - height[left];
                ++left;
            } else {
                ans += rightMax - height[right];
                --right;
            }
        }
        return ans;
    }
};
```

