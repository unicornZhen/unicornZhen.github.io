# 332.重新安排行程

[题目链接](https://leetcode.cn/problems/reconstruct-itinerary/description/)

### 思路

> 方法一：蛮力的回溯法。剪枝：通过对边的排序预处理，找到第一个解即为题目所求的解。
>
> 方法二：找欧拉通路或者欧拉回路的回溯法。关于此算法见[leetcode官方题解](https://leetcode.cn/problems/reconstruct-itinerary/solutions/389885/zhong-xin-an-pai-xing-cheng-by-leetcode-solution/)，对于方法二暂时没有去仔细理解，暂且给它标记为一个找欧拉通路或者回路的算法，记住并且能够应用即可。
>
> 方法一在倒数第二个测例超时，时间复杂度高，将点的名字映射成数字可以进一步降低运行的时间，可以通过测试。方法二高效。

### 时空复杂度

> m为tickets的长度
>
> 方法一的时间复杂度不便计算，空间复杂度O(m)。
>
> 方法二的时间复杂度为$$mlogm$$,空间复杂度为O(m)。

### 源码

**方法一源码**

```
class Solution {
public:
    vector<string> findItinerary(vector<vector<string>> &tickets) {
        unordered_map<string, vector<string>> graph_tickets;
        unordered_map<string, int> visited;
        for (auto &it: tickets) {
            graph_tickets[it[0]].push_back(it[1]);
            if (visited.find(it[0] + it[1]) == visited.end()) {
                visited[it[0] + it[1]] = 1;
            } else {
                visited[it[0] + it[1]]++;
            }
        }

        for (auto &it: graph_tickets) {
            sort(it.second.begin(), it.second.end());
        }

        vector<string> result;
        dfs(result, graph_tickets, visited, 0, tickets);
        return result;
    }

private:
    bool dfs(vector<string> &result, const unordered_map<string, vector<string>> &graph_tickets,
             unordered_map<string, int> &visited, int visited_edges, const vector<vector<string>> &tickets) {
        //boundary condition
        if (visited_edges >= tickets.size()) {
            return true;
        }

        // start dfs
        string start;
        if (result.empty()) {
            result.emplace_back("JFK");
        }
        start = result.back();
        // pruning
        if (graph_tickets.find(start) == graph_tickets.end()) {
            return false;
        }
        for (auto &it: graph_tickets.at(start)) {
            if (visited[start + it] > 0) {
                result.push_back(it);
                visited[start + it]--;
                if (dfs(result, graph_tickets, visited, visited_edges + 1, tickets)) {
                    return true;
                }
                visited[start + it]++;
                result.pop_back();
            }
        }
        return false;
    }
};
```

**方法二的源码**

```
class Solution {
public:
    unordered_map<string, priority_queue<string, vector<string>, std::greater<string>>> vec;

    vector<string> stk;

    void dfs(const string& curr) {
        while (vec.count(curr) && vec[curr].size() > 0) {
            string tmp = vec[curr].top();
            vec[curr].pop();
            dfs(move(tmp));
        }
        stk.emplace_back(curr);
    }

    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for (auto& it : tickets) {
            vec[it[0]].emplace(it[1]);
        }
        dfs("JFK");

        reverse(stk.begin(), stk.end());
        return stk;
    }
};
```

# 51.N皇后、37.解数独

[N皇后题目链接](https://leetcode.cn/problems/n-queens/description/)、[解数独题目链接](https://leetcode.cn/problems/sudoku-solver/description/)

### 简介

> 之前做过，由于时间关系在此不再重新做

### 源码

**51.N皇后源码**

```
class Solution {
 public:
	vector<vector<string>> solveNQueens(int n) {
		total=n;
		for(int i=0;i<n;i++){
			pos.emplace_back(-1);
			col.emplace_back(1);
		}
		for(int i=0;i<2*n-1;i++){
			right_incline.emplace_back(1);
			left_incline.emplace_back(1);
		}
		dfs(0);
		return result;
	}
	void dfs(int n){
		// 边界条件
		if(n==total){
			vector<string> sol;
			string tmp(total,'.');
			for(int i=0;i<total;i++){
				sol.emplace_back(tmp);
			}
			for(int i=0;i<pos.size();i++){
				sol[i][pos[i]]='Q';
			}
			result.emplace_back(sol);
			return;
		}
			// dfs主体
		else{
			for(int i=0;i<total;i++){
				if(col[i]==1&&right_incline[i-n+total-1]==1&&left_incline[i+n]==1){
					// 竖、斜都标记为false(横不用管）
					pos[n]=i;
					col[i]=0;
					right_incline[i-n+total-1]=0;
					left_incline[i+n]=0;
					dfs(n+1);
					col[i]=1;
					right_incline[i-n+total-1]=1;
					left_incline[i+n]=1;
				}
			}
			return;
		}
	}
 private:
	vector<int> pos,col,right_incline,left_incline;
	vector<vector<string>> result;
	int total;
};
```

**37.解数独源码**

```
class Solution {
private:
    bool line[9][9];
    bool column[9][9];
    bool block[3][3][9];
    bool valid;
    vector<pair<int, int>> spaces;

public:
    void dfs(vector<vector<char>>& board, int pos) {
        if (pos == spaces.size()) {
            valid = true;
            return;
        }

        auto [i, j] = spaces[pos];
        for (int digit = 0; digit < 9 && !valid; ++digit) {
            if (!line[i][digit] && !column[j][digit] && !block[i / 3][j / 3][digit]) {
                line[i][digit] = column[j][digit] = block[i / 3][j / 3][digit] = true;
                board[i][j] = digit + '0' + 1;
                dfs(board, pos + 1);
                line[i][digit] = column[j][digit] = block[i / 3][j / 3][digit] = false;
            }
        }
    }

    void solveSudoku(vector<vector<char>>& board) {
        memset(line, false, sizeof(line));
        memset(column, false, sizeof(column));
        memset(block, false, sizeof(block));
        valid = false;

        for (int i = 0; i < 9; ++i) {
            for (int j = 0; j < 9; ++j) {
                if (board[i][j] == '.') {
                    spaces.emplace_back(i, j);
                }
                else {
                    int digit = board[i][j] - '0' - 1;
                    line[i][digit] = column[j][digit] = block[i / 3][j / 3][digit] = true;
                }
            }
        }

        dfs(board, 0);
    }
};
```

## 总结

* 对于欧拉通路和回路的定义以及相对应的算法，从第一题中所学习得到。
* 使用unordered_map定义邻接表。
* 回溯是递归的产物，只要有递归就会有回溯