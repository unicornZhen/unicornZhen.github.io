# 层序遍历：102、429、637
[102题目链接](https://leetcode.cn/problems/binary-tree-level-order-traversal/)、[429题目链接](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/submissions/497920460/)、[637题目链接](https://leetcode.cn/problems/average-of-levels-in-binary-tree/submissions/497917290/)
## 思路
二叉树层序遍历的基本思路和扩展。值得注意的是二叉树的各层划分的方法，利用一些变量在入队时记住每层的大小。
## 时空复杂度
O(n)、S(n)
### 源码
637的源码：
``` 
struct TreeNode {  
    int val;  
    TreeNode *left;  
    TreeNode *right;  
  
    TreeNode() : val(0), left(nullptr), right(nullptr) {}  
  
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}  
  
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}  
};  
  
class Solution {  
public:  
    vector<double> averageOfLevels(TreeNode *root) {  
        vector<double> result;  
  
        int level_left_nodes=1;  
        int level_next_nodes=0;  
        int level_cur_nodes=1;  
        double sum=0;  
        queue<TreeNode *> que;  
        TreeNode *p;  
  
        que.push(root);  
        while(!que.empty()){  
            p=que.front();  
            que.pop();  
            sum+=p->val;  
            if(p->left){  
                que.push(p->left);  
                level_next_nodes++;  
            }  
            if(p->right){  
                que.push(p->right);  
                level_next_nodes++;  
            }  
            level_left_nodes--;  
            if(level_left_nodes<=0){  
                level_left_nodes=level_next_nodes;  
                level_cur_nodes=level_next_nodes;  
                level_next_nodes=0;  
                sum=0;  
                // 按照规定格式计算出这层节点的平均值  
                result.push_back(sum/level_cur_nodes);  
            }  
        }  
        return result;  
    }  
};
```

# 226.反转二叉树、101.对称二叉树
[226题目链接](https://leetcode.cn/problems/invert-binary-tree/description/)、[101题目链接](https://leetcode.cn/problems/symmetric-tree/)
## 思路

就很简单的递归

## 时空复杂度
O(n)、logn
### 源码
226源码

```
/**  
 * Definition for a binary tree node. * struct TreeNode { *     int val; *     TreeNode *left; *     TreeNode *right; *     TreeNode() : val(0), left(nullptr), right(nullptr) {} *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {} *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {} * }; */class Solution {  
public:  
    TreeNode* invertTree(TreeNode* root) {  
        if(root==nullptr){  
            return nullptr;  
        }  
        swap(root->left,root->right);  
        invertTree(root->left);  
        invertTree(root->right);  
        return root;  
    }  
};
```
