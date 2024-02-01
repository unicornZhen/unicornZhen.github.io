# 530.二叉搜索树的最小绝对值差

[题目链接](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/description/)

### 思路

> 很简单，利用递归的中序遍历或者迭代的中序遍历都可以做，本题采用迭代的中序遍历方法。

### 时空复杂度

> O(n)、S(n)

### 源码

```
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        stack<TreeNode*> nodeStack;
        const int MAX_VALUE=10e5;
        TreeNode *p=root;
        int pre_val=3*10e5;
        int result=INT_MAX;
        while (p || !nodeStack.empty()) {
            while (p) {
                // 将左子树节点入栈
                nodeStack.push(p);
                p = p->left;
            }

            // 出栈访问节点
            p = nodeStack.top();
            nodeStack.pop();
            if(fabs(p->val-pre_val)<result){
                result=fabs(p->val-pre_val);
            }
            pre_val=p->val;

            // 访问右子树
            p = p->right;
        }
        return result;
    }
};
```



# 501.在BST中找众数

[题目链接](https://leetcode.cn/problems/find-mode-in-binary-search-tree/description/)

### 思路

与530的解题思路类似，同样借鉴二叉树的中序遍历，代码写得有些丑陋和冗余，懒得修改了。

### 时空复杂度

O(n)、S(n)

### 源码

```
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> nodeStack;
        const int MAX_VALUE=10e5+1;
        TreeNode *p=root;
        int pre_val=MAX_VALUE;
        int max_show_times=0;
        int show_times=1;
        while (p || !nodeStack.empty()) {
            while (p) {
                nodeStack.push(p);
                p = p->left;
            }
            p = nodeStack.top();
            nodeStack.pop();

            if(p->val==pre_val){
                show_times++;
                // 处理第一个节点
            }else if(pre_val==MAX_VALUE){
                show_times=1;
            }else{
                if(show_times>max_show_times){
                    result.clear();
                    result.push_back(pre_val);
                    max_show_times=show_times;
                }else if(show_times==max_show_times){
                    result.push_back(pre_val);
                }
                show_times=1;
            }
            pre_val=p->val;

            p = p->right;
        }
        // 处理最后一个节点
        if(show_times>max_show_times){
            result.clear();
            result.push_back(pre_val);
            max_show_times=show_times;
        }else if(show_times==max_show_times){
            result.push_back(pre_val);
        }
        return result;
    }
};
```



# 236.二叉树的最近公共祖先

[题目链接](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)

### 思路

设这两个节点为p和q

方法一：先得到p和q的路径（dfs或者迭代的后序遍历），在根据这两条路径找到最近公共祖先。

方法二：直接根据题目递归得出最近公共祖先，相比一更快，扫描一遍树的节点即可得出答案。

本题实现采用方法一，方法一比较简单不再赘述。值的注意的是方法二，方法二的设计比较巧妙，递归的自底向上的回溯保证得到的答案是最近公共祖先。

### 时空复杂度

两种方法的时空复杂度都是O(n)、S(n)

### 源码

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr || root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left == nullptr) return right;
        if (right == nullptr) return left;
        return root;
    }
};
```

