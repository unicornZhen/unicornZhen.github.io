# <span style="color:red">239.滑动窗口的最大值</span>
[题目链接](https://leetcode.cn/problems/sliding-window-maximum/description/)
### 思路
方法一：优先队列。需要注意优先队列的实现，priority_queue<pair<int,int>>的比较函数会默认比较第一个，也可以priority_queue<element,比较函数>按照priority_queue的模板声明来用。

方法二：单调队列。与单调栈类似，由于我们需要求出的是滑动窗口的最大值，如果当前的滑动窗口中有两个下标 i和 j，其中 i在 j 的左侧（i<<j），并且 i对应的元素不大于 j 对应的元素（nums[i]≤nums[j]），那么会发生什么呢？nums[i] 一定不会是滑动窗口中的最大值了，我们可以将 nums[i]。因此我们可以使用一个队列存储所有还没有被移除的下标。在队列中，这些下标按照从小到大的顺序被存储，并且它们在数组 nums 中对应的值是严格单调递减的。

### 时空复杂度
方法一:O(nlogn),S(n)
方法二：O(n),S(k)
### 源码
```
class Solution {  
public:  
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {  
        int n = nums.size();  
        priority_queue<pair<int, int>> q;  
        for (int i = 0; i < k; ++i) {  
            q.emplace(nums[i], i);  
        }  
        vector<int> ans = {q.top().first};  
        for (int i = k; i < n; ++i) {  
            q.emplace(nums[i], i);  
            while (q.top().second <= i - k) {  
                q.pop();  
            }  
            ans.push_back(q.top().first);  
        }  
        return ans;  
    }  
};
```

# 347.Top K Frequent Elements
[题目链接](https://leetcode.cn/problems/top-k-frequent-elements/description/)
### 思路
方法一：先利用unordered_map（哈希表)统计nums中各个元素的频率，利用小根堆实现找到这个unorderd_map中的前k个元素，值得注意的是compare函数的编写。
方法二：快排找前k个最大元素。
本题采用方法一
### 时空复杂度
O(nlogk)、S(n)
### 源码
```
class Solution {  
public:  
    vector<int> topKFrequent(vector<int>& nums, int k) {  
        // 方法：先统计出各个元素的频率,然后再使用大根堆得到最大值  
        struct Compare {  
            bool operator()(const pair<int, int>& a, const pair<int, int>& b) {  
                return a.first > b.first;  
            }  
        };  
  
        vector<int> result;  
        priority_queue<pair<int,int>,vector<pair<int,int>>,Compare> priorityQueue;//pair<the frequency of the element,the value of the element>  
        unordered_map<int,int> frequency;//<the value of the element,the frequency of the element>  
        for(auto num:nums){  
            frequency[num]++;  
        }  
  
        for(auto it:frequency){  
            priorityQueue.push({it.second,it.first});  
            if(priorityQueue.size()>=k+1){  
                priorityQueue.pop();  
            }  
        }  
  
        while (!priorityQueue.empty()){  
            result.push_back(priorityQueue.top().second);  
            priorityQueue.pop();  
        }  
        return result;  
    }  
};
```

# 总结
* 单调栈和单调队列的思想十分巧妙，其应用要尽量把握。
* 优先级队列的使用：滑动窗口、前k个元素，还有compare、pair<int,int>的使用和编写。
