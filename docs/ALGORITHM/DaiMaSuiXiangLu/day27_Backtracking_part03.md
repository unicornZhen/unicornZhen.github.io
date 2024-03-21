# 39.组合总和

[题目链接](https://leetcode.cn/problems/combination-sum/description/)

### 思路

> 回溯法，简单的dfs。注意path中值是每个candidate的选择情况，为了减少入栈和出栈的操作次数。

### 时空复杂度

> 设n为candidate的长度
>
> 时间复杂度不便计算，空间复杂度为S(n)

### 源码

```C++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int> &candidates, int target) {
        vector<vector<int>> result;
        vector<int> path(candidates.size(), 0);
        dfs(result, candidates, target, path, 0);
        return result;
    }

private:
    void dfs(vector<vector<int>> &result, const vector<int> &candidates, int target, vector<int> &path, int start) {
        if (target == 0) {
            vector<int> tmp;
            for (int i = 0; i < path.size(); i++) {
                int nums=path[i];
                while (nums > 0) {
                    tmp.push_back(candidates[i]);
                    nums--;
                }
            }
            if (!tmp.empty()) {
                result.push_back(tmp);
            }
            return;
        }
        if (target < 0 || start < 0 || start >= candidates.size()) {
            return;
        }

        for (int i = 0; i * candidates[start] <= target; i++) {
            path[start] = i;
            dfs(result, candidates, target - i * candidates[start], path, start + 1);
            path[start] = 0;
        }
    }
};
```

# 40.组合总和II

[题目链接](https://leetcode.cn/problems/combination-sum-ii/description/)

### 思路

> 与39思路类似，在39的题目解答上进行一些调整即可

### 时空复杂度

> 设n为candidates的长度
>
> 时间复杂度不便分析，空间复杂度为O(n)

### 源码

```C++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int> &candidates, int target) {
        sort(candidates.begin(),candidates.end());
        vector<vector<int>> result;
        vector<int> path;
        dfs(result, candidates, target, path, 0);
        return result;
    }

private:
    void dfs(vector<vector<int>> &result, const vector<int> &candidates, int target, vector<int> &path, int start) {
        // Boundary conditions
        if (target <= 0 || start < 0 || start >= candidates.size()) {
            if(target==0){
                result.push_back(path);
            }
            return;
        }

        // get the start and end if there is the same elements in candidates in start index
        int end=start;
        while(end<candidates.size()&&candidates[end]== candidates[start]){
            end++;
        }
        end--;

        // start dfs
        int tmp;
        for(int i=0;i<=end-start+1;i++){
            tmp=i;
            while(tmp>0){
                path.push_back(candidates[start]);
                tmp--;
            }
            dfs(result,candidates,target-i*candidates[start],path,end+1);
            tmp=i;
            while (tmp>0){
                path.pop_back();
                tmp--;
            }
        }
    }
};
```

# 131.分割回文串

[题目链接](https://leetcode.cn/problems/palindrome-partitioning/)

### 思路

> 方法一：回溯法。比较简单，注意可以通过对s的预处理使得对一个字符串的字串判断是否为回文串降为O(1)时间复杂度。
>
> 方法二：dp法。dp[i]为s[0,i]的所有为回文串的划分方法，转移方程较为简单，但使用markdown编写较为麻烦，在此省略。
>
> 本题采用回溯法。

### 源码

```C++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> result;
        vector<string> path;
        dfs(result,s,0,path);
        return result;
    }
private:
    void dfs(vector<vector<string>>& result,const string &s,int start,vector<string> &path){
        if(start>=s.size()){
            result.push_back(path);
        }

        for(int i=start;i<s.size();i++){
            if(isPalindrome(s,start,i)){
                path.push_back(s.substr(start,i-start+1));
                dfs(result,s,i+1,path);
                path.pop_back();
            }
        }
    }
    bool isPalindrome(const string &s, int start,int end){
        if(start<0||start>end||end>=s.size()){
            return false;
        }
        for(int i=start;i<start+(end-start+1)/2;i++){
            if(s[i]!=s[end-i+start]){
                return false;
            }
        }
        return true;
    }
};
```

## 总结

* 回溯的进阶使用，使用回溯框架思考问题和写代码
* 注意优化，预处理、dp等，例如在131中对s进行dp的预处理。