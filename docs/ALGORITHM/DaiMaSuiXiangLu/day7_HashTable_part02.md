# 454.四数之和
### 思路
四个数组,长度都为O（n），利用hash_map储存其中两个数组的组合，需要O(n2)的复杂度，再遍历剩下的两个数组查询hash_map中是否存在其组合和的相反数，注意hash_map中的value是组合的和个数，需要统计重复的元素。
### 时空复杂度
O(n2):四个数组分成两组，每组各自迭代组合扫描一遍，需要O(n2)
S(n2):hash_map需要n2的空间
### 源码
```
class Solution {  
public:  
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {  
        int result=0;  
        unordered_map<int,int> map_12;  
        for(auto it1:nums1){  
            for(auto it2:nums2){  
                if(map_12.find(it1+it2)==map_12.end()) {  
                    map_12.insert({it1 + it2, 1});  
                }else{  
                    map_12[it1+it2]++;  
                }  
            }  
        }  
  
        for(auto it1:nums3){  
            for(auto i2:nums4){  
                if(map_12.find(-(it1+i2))!=map_12.end()){  
                    result+=map_12[-(it1+i2)];  
                }  
            }  
        }  
        return result;  
    }  
};
```

# 383.赎金信
### 思路
与242.有效的字母异位词思路一样

### 时空复杂度
O(n)
S(n)
### 源码
```
class Solution {  
public:  
    bool canConstruct(string ransomNote, string magazine) {  
        unordered_map<char,int> m_ransomNote;  
        unordered_map<char,int> m_magazine;  
        for(auto it:ransomNote){  
            m_ransomNote[it]++;  
        }  
        for(auto it:magazine){  
            m_magazine[it]++;  
        }  
  
        for(auto it:m_ransomNote){  
            if(m_magazine.find(it.first)==m_magazine.end()||m_magazine[it.first]<it.second){  
                return false;  
            }  
        }  
        return true;  
    }  
};
```

# 15.三数之和
### 思路
参考两数之和的思路，先对数组排序，固定一个数，剩余两个数的查找使用两数之和的思路。值得注意的是对于重复元组排除的技巧。
### 时空复杂度
O(nlogn+n2)=O(n2):两层for循环。
O(logn):取决与排序的复杂度，这里采用sort，也就是快排的复杂度。
### 源码
```
class Solution {  
public:  
    vector<vector<int>> threeSum(vector<int>& nums) {  
        sort(nums.begin(),nums.end());  
        vector<vector<int>> result;  
        size_t start=0;  
        size_t end=nums.size()-1;  
        for(int i=0;i<nums.size()-2;i++){  
            start=i+1;  
            end=nums.size()-1;  
            if(!result.empty()&&result[result.size()-1][0]==nums[i]){  
                continue;  
            }  
            while (start<end){  
                if(nums[start]+nums[end]+nums[i]==0){  
                    result.push_back({nums[i],nums[start],nums[end]});  
                    size_t tmp=start;  
                    while (start<end&&nums[tmp]==nums[start]){  
                        start++;  
                    }   
                    end--;  
                }else if(nums[start]+nums[end]+nums[i]<0){  
                    start++;  
                }else{  
                    end--;  
                }  
            }  
        }  
        return result;  
    }  
};
```

# 454.四数之和
### 思路
四数之和降为三数之和，再降为两数之和，同样注意重复结果数组的处理，需要特别注意溢出处理，在nums[i]+nums[j]+nums[k]+nums[m]的过程要防止溢出。
### 时空复杂度
O(n3):三层循环
S(1)
### 源码
```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int> &nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < (int) nums.size() - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < (int) nums.size() - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                int start = j + 1;
                int end = nums.size() - 1;
                while (start < end) {
                    long long sum = (long long) nums[i] + nums[j] + nums[start] + nums[end];
                    if (sum == target) {
                        result.push_back({nums[i], nums[j], nums[start], nums[end]});
                        int tmp = start;
                        while (start < end && nums[start] == nums[tmp]) {
                            start++;
                        }
                        end--;
                    } else if (sum < target) {
                        start++;
                    } else {
                        end--;
                    }
                }
            }
        }
        return result;
    }
};
```

