# 491.非递减子序列

[题目链接](https://leetcode.cn/problems/non-decreasing-subsequences/description/)

### 思路

> 回溯法。特别注意去重的处理，这个可以极大优化程序的运行时间。
>
> 有两种去重的方法，第一种是使用last记录前一个选择的元素，具体见代码。若有两个相同的元素，选择第一个和不选择第二个与不选择第一个和选择第二个这两种将导致重复结果，通过与last比较只保留第二种从而达到去掉重复的操作。
>
> 第二种是给每个合法解的序列生成一个hash值，重点是hash函数的设计，具体可以见[leetcode官方解答](https://leetcode.cn/problems/non-decreasing-subsequences/solutions/387656/di-zeng-zi-xu-lie-by-leetcode-solution/)。
>
> 本题采用第一种方法，其中第一种方法的去重参考答案编写。

### 时空复杂度

> O($$n*2^n$$),S(n)

### 源码

```
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int> &nums) {
        vector<vector<int>> result;
        vector<int> path;
        dfs(result, nums, path, 0, INT32_MIN);
        return result;
    }

private:
    void dfs(vector<vector<int>> &result, const vector<int> &nums, vector<int> &path, int cur, int last) {
        //boundary condition:find a valid solution
        if (cur < 0) {
            return;
        }
        if (cur >= nums.size()) {
            if(path.size()>=2){
                result.push_back(path);
            }
            return;
        }

        // start dfs
        if (nums[cur] != last) {
            dfs(result, nums, path, cur + 1, last);
        }

        if (nums[cur] >= last) {
            path.push_back(nums[cur]);
            dfs(result, nums, path, cur + 1, nums[cur]);
            path.pop_back();
        }
    }
};
```

# 46.全排列

[题目链接class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> result;
        vector<int> path;
        vector<bool> visited(nums.size(),false);
        dfs(result,path,nums,visited);
        return result;
    }
private:
    void dfs(vector<vector<int>> &result,vector<int> &path,const vector<int> &nums,vector<bool> &visited){
        // boundary conditions
        if(path.size()>=nums.size()){
            result.push_back(path);
        }
        
        // start dfs
        int pre_val=INT32_MIN;
        for(int i=0;i<nums.size();i++){
            if(!visited[i]&&nums[i]!=pre_val){
                pre_val=nums[i];
                path.push_back(nums[i]);
                visited[i]=true;
                dfs(result,path,nums,visited);
                visited[i]=false;
                path.pop_back();
            }
        }
    }
};](https://leetcode.cn/problems/permutations/description/)

### 思路

> 典型的回溯问题，仿照排列的定义与获取编写回溯。具体见源代码。

### 时空复杂度

> 设n为数组长度
>
> O(n!),S(n)

### 源码

```
class Solution {
public:
    vector<vector<int>> permute(vector<int> &nums) {
        vector<vector<int>> result;
        vector<bool> visited(nums.size(), false);
        vector<int> path;
        dfs(result, visited, nums, path);
        return result;
    }

private:
    void dfs(vector<vector<int>> &result, vector<bool> &visited, const vector<int> &nums, vector<int> &path) {
        // boundary conditions
        if (path.size() >= nums.size()) {
            result.push_back(path);
        }

        // start dfs
        for (int i = 0; i < nums.size(); i++) {
            if (!visited[i]) {
                path.push_back(nums[i]);
                visited[i] = true;
                dfs(result, visited, nums, path);
                visited[i] = false;
                path.pop_back();
            }
        }
    }
};
```

# 47.全排列II

[题目链接](https://leetcode.cn/problems/permutations-ii/description/)

### 思路

> 在全排列一的基础上增加了去重的操作，较为简单，具体见下列源码。

### 时空复杂度

> n为nums的长度
>
> O(n!)，S(n)

### 源码

```
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> result;
        vector<int> path;
        vector<bool> visited(nums.size(),false);
        dfs(result,path,nums,visited);
        return result;
    }
private:
    void dfs(vector<vector<int>> &result,vector<int> &path,const vector<int> &nums,vector<bool> &visited){
        // boundary conditions
        if(path.size()>=nums.size()){
            result.push_back(path);
        }
        
        // start dfs
        int pre_val=INT32_MIN;
        for(int i=0;i<nums.size();i++){
            if(!visited[i]&&nums[i]!=pre_val){
                pre_val=nums[i];
                path.push_back(nums[i]);
                visited[i]=true;
                dfs(result,path,nums,visited);
                visited[i]=false;
                path.pop_back();
            }
        }
    }
};
```

## 总结

* 主要是第一题中的去掉重复的操作：为每个序列生成hash值，判断新的序列是否有已经存在的对应hash值，关键是hash函数的设计；第二点是巧妙使用last变量。
