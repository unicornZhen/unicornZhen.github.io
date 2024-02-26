# 93.复原IP地址

[题目链接](https://leetcode.cn/problems/restore-ip-addresses/description/)

### 思路

> 方法一：回溯法。一层一层地找切分点，可以针对任何数量的切分点。
>
> 方法二：非回溯法。由于经分析可知只需要找三个切分点，故使用三个for循环解决。
>
> 本题采用方法二

### 时空复杂度

> n为s的长度
>
> 不好分析时间复杂度，空间复杂度为S(1)

### 源码

```
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> result;
        int len=s.size();
        for(int i=0;i<len-3;i++){
            if(!isValidIp(s,0,i)){
                continue;
            }
            for(int j=i+1;j<len-2;j++){
                if(!isValidIp(s,i+1,j)){
                    continue;
                }
                for(int k=j+1;k<len-1;k++){
                    if(isValidIp(s,j+1,k)&& isValidIp(s,k+1,len-1)){
                        string tmp=s.substr(0,i+1)+"."+s.substr(i+1,j-i)+"."+
                                s.substr(j+1,k-j)+"."+s.substr(k+1,len-1-k);
                        result.push_back(tmp);
                    }
                }
            }
        }
        return result;
    }
private:
    bool isValidIp(string s,int start,int end){
        if(start<0||end>=s.size()){
            return false;
        }
        for(int i=start;i<end;i++){
            if(!isdigit(s[i])){
                return false;
            }
        }
        if(end-start>2||end-start<0){
            return false;
        }

        int dig_ip= stoi(s.substr(start,end-start+1));

        switch (end-start) {
            case 2:
                if(dig_ip<100||dig_ip>255){
                    return false;
                }
                break;
            case 1:
                if(dig_ip<10){
                    return false;
                }
                break;
        }
        return true;
    }
};

```

# 78.子集

[题目链接](https://leetcode.cn/problems/subsets/description/)

### 思路

> 方法一：回溯法。很简单的回溯法。
>
> 方法二：非回溯法。子集的个数为$$2^n$$,每个子集可映射为0到$$2^n$$的一个数字，具体见源码。

### 时空复杂度

> S($$2^n$$),S(1)

### 源码

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int> &nums) {
        int total = pow(2, nums.size());
        vector<vector<int>> result(total);
        for (int i = 0; i < total; i++) {
            int tmp = i;
            for (int j = 0; j < nums.size(); j++) {
                if (tmp % 2 == 1) {
                    result[i].push_back(nums[j]);
                }
                tmp >>= 1;
            }
        }
        return result;
    }
};

```

# 90.子集II

[题目链接](https://leetcode.cn/problems/subsets-ii/description/)

### 思路

> 回溯法。有两种设计思路，第一种：决策途径为选择0个元素->选择一个元素->选择二个元素->...->选择n个元素。，故决策树中的每个节点都是一个解。第二种：决策是是否第一个元素->是否选择第二个元素->...->选择第n个元素，故决策树中每个叶子节点是一个解。
>
> 本题采用第二种解答方法，第二种方法需要探索的状态更少，但时间复杂度还是一样。

### 时空复杂度

> O($$2^n$$),S(n)

### 源码

```
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        vector<int> path;
        dfs(result, nums, path, 0);
        return result;
    }

private:
    void dfs(vector<vector<int>> &result, const vector<int> &nums, vector<int> &path, int start) {
        //boundary condition:find a valid solution
        if(start<0){
            return;
        }
        if (start >= nums.size()) {
            result.push_back(path);
            return;
        }
        
        // start dfs
        int end=start;
        while(end<nums.size()&&nums[end]==nums[start]){
            end++;
        }
        
        int tmp;
        for(int i=0;i<=end-start;i++){
            tmp=i;
            while(tmp>0){
                path.push_back(nums[start]);
                tmp--;
            }
            dfs(result,nums,path,end);
            tmp=i;
            while (tmp>0){
                path.pop_back();
                tmp--;
            }
        }
    }
};
```

