# 242.有效的异位字符串
### 思路
利用哈希表判断两个字符串所含的同样类型的字符数量是否一样
### 时空复杂度
O(n+m):对于两个字符串各扫描一遍
S(1):小写字母的个数的常数空间
### 源码
```C++
class Solution {  
public:  
    bool isAnagram(string s, string t) {  
        unordered_map<char,int> m_s;  
        unordered_map<char,int> m_t;  
        for(auto it:s){  
            m_s[it]++;  
        }  
        for(auto it:t){  
            m_t[it]++;  
        }  
        return m_s==m_t;  
    }  
};
```

# 349.两个数组的交集
### 思路
设这两个数组为n1,n2。先使用unoreded_set的set（底层用hash是实现的）存n1中的元素同时可以去重，在遍历n2看是否在set中出现，出现则为两个数组相交，注意处理结果中的重复元素。
### 时空复杂度：
O(n1+n2):各自扫描一遍，假设hash的查找速度为O(1)
S(n1):set存num1中的内容
### 源码
```C++
class Solution {  
public:  
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {  
        unordered_set<int> s_nums1;  
        vector<int> result;  
        for(auto it:nums1){  
            s_nums1.insert(it);  
        }  
        for(auto it:nums2){  
            if(s_nums1.find(it)!=s_nums1.end()){  
                result.push_back(it);  
                s_nums1.erase(it);  
            }  
        }  
        return result;  
    }  
};
```

# 202.快乐数字
### 思路
发现快乐数字的变化过程中产生的新的数字的个数是有限的，设n的位数为rank,则新数字的个数上界为81\*rank。
### 时空复杂度
O(81\*rank):基本处于O(1状态)
S(81\*rank)
### 源码
```C++
class Solution {  
public:  
    bool isHappy(int n) {  
        if(n==1)return true;  
        unordered_set<int> s;  
        s.insert(n);  
        int sum=0;  
        while(n!=1){  
            while(n!=0){  
                sum+=(n%10)*(n%10);  
                n/=10;  
            }  
            if(s.find(sum)!=s.end())return false;  
            s.insert(sum);  
            n=sum;  
        }  
        return true;  
    }  
};
```

# 1.两数之和
### 思路
可以先排序再双指针or使用hash做，这里说使用hash做
### 时空复杂度
O(n):需要扫描一趟
S(n):使用hash存已经扫描的数字
### 源码
```C++
class Solution {  
public:  
    vector<int> twoSum(vector<int> &nums, int target) {  
        unordered_map<int,int> s;  
        for(int i=0;i<nums.size();i++){  
            if(s.find(target-nums[i])!=s.end()){  
                return {s[target-nums[i]],i};  
            }  
            s[nums[i]]=i;  
        }  
        return {};  
    }  
};
```