# 216.组合和III

[题目链接](https://leetcode.cn/problems/combination-sum-iii/description/)

### 思路

> 简单的dfs，特别注意的是剪枝，两个剪枝点：一是下一次递归中的起点是上一次的i+1；剩下待k和n的与总的k和n之间的关系剪枝。

### 时空复杂度

> O(k!),S(k!)

### 源码

```

class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> result;
        vector<int> path;
        dfs(result, path,1, n,k);
        return result;
    }

private:
    void dfs(vector<vector<int>>& result, vector<int>& path, 
    int start, int n,int k) {
        if(k<=0||n<0){
            if(k==0&&n==0){
                result.push_back(path);
            }
            return;
        }

        for(int i=start;i<=9;i++){
            path.push_back(i);
            dfs(result,path,i+1,n-i,k-1);
            path.pop_back();
        }
    }
};
```

# 17.电话号码的组合

[题目链接](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)

### 思路

> 方法一：回溯法，很简单的使用回溯。
>
> 方法二：循环替代回溯，降低空间复杂度为常熟级别。
>
> 本次是第二次解答这个题目，采用方法二解答；第一次是采用方法一解答。

### 时空复杂度

> 设n为digit中数字为2～6的个数，m为数字是7～9的个数
>
> 方法一：O($$3^n+4^m$$)，S(n+m)
>
> 方法二：O($$3^n+4^m$$)，S(1)

### 源码

```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<vector<char>> table = {{'a', 'b', 'c'},
                                      {'d', 'e', 'f'},
                                      {'g', 'h', 'i'},
                                      {'j', 'k', 'l'},
                                      {'m', 'n', 'o'},
                                      {'p', 'q', 'r', 's'},
                                      {'t', 'u', 'v'},
                                      {'w', 'x', 'y', 'z'}};
        vector<string> result;
        for(auto it:digits){
            int pos=it-'2';
            if(result.empty()){
                for(auto it1:table[pos]){
                    string tmp(1,it1);
                    result.push_back(tmp);
                }
                continue;
            }
            int len=result.size();
            for(int i=0;i<len;i++){
                for(int j=1;j<table[pos].size();j++){
                    result.push_back(result[i]+table[pos][j]);
                }
                result[i]+=table[pos][0];
            }
        }
        return result;
    }
};
```

## 总结

* 简单的使用回溯，还是得注意剪枝的使用
