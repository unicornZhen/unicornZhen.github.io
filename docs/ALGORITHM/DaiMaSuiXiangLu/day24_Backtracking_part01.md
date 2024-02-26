

## 回溯的模板

接下来，我们尝试将回溯的“尝试、回退、剪枝”的主体框架提炼出来，提升代码的通用性。 在以下框架代码中，state 表示问题的当前状态，choices 表示当前状态下可以做出的选择：

```
/* 回溯算法框架 */ 
void backtrack(State *state, vector<Choice *> &choices, vector<State *> &res) { 
	// 判断是否为解 
	if (isSolution(state)) { 
		// 记录解 recordSolution(state, res); 
		// 不再继续搜索 return; 
	} 
	// 遍历所有选择 
	for (Choice choice : choices) { 
		// 剪枝：判断选择是否合法 
		if (isValid(state, choice)) { 
			// 尝试：做出选择，更新状态 
			makeChoice(state, choice); 
			backtrack(state, choices, res); 
			// 回退：撤销选择，恢复到之前的状态 
			undoChoice(state, choice); 
		} 
	} 
}
```

# 77.组合

[题目链接](https://leetcode.cn/problems/combinations/description/)

### 思路

> 简单而典型的回溯问题。

### 时空复杂度

> O($$C_n^k$$),S(k)

### 源码

```
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> result;
        vector<int> path;
        dfs(n,k,1,path,result);
        return result; 
    }
private:
    void dfs(int n,int k,int start,vector<int> &path,vector<vector<int>> &result){
        if(k<=0){
            result.push_back(path);
            return;
        }

        k--;
        for(int i=start;i<=n-k;i++){
            path.push_back(i);
            dfs(n,k,i+1,path,result);
            path.pop_back();
        }
    }
};
```

