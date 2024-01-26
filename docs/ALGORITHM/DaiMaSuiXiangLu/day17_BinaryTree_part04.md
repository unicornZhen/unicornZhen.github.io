# 110.平衡二叉树的判定

[题目链接](https://leetcode.cn/problems/balanced-binary-tree/description/)

### 思路

方法一：递归遍历。写一个既可以计算高度和是否是平衡树的函数，将蛮力法的时间复杂度从O(n<sup>2</sup>)降为O(n)

方法二：迭代遍历。改写二叉树的层序遍历，自底向上层序遍历二叉树，设计一个数据结构记录当前层的高度，比较麻烦。

本题采用的是方法一。

### 时空复杂度

O(n)、S(n)

### 源码

```
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(myIsBalanced(root)>=0){
            return true;
        }
        return false;
    }
private:
    // -1代表不是平衡二叉树，其他非负数代表树的高度且是平衡二叉树
    int myIsBalanced(TreeNode *root){
        if(root==nullptr){
            return 0;
        }
        int l_height=myIsBalanced(root->left);
        int r_height=myIsBalanced(root->right);
        if(r_height<0||l_height<0||fabs(r_height-l_height)>1){
            return -1;
        }
        return max(r_height,l_height)+1;
    }
};
```

# 257.二叉树路径

[题目链接](https://leetcode.cn/problems/binary-tree-paths/)

### 思路

方法一：递归法。在dfs的基础上找路径，比较简单

方法二：迭代法。利用二叉树的迭代后序遍历中当前栈是当前访问节点的从根到当前访问节点的路径这个特点。

本题采用方法一，鉴于时间因素没有实现方法二。

### 时空复杂度

O(n)、S(n)

### 源码

```
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        string path=to_string(root->val);
        if(root->left==nullptr&&root->right==nullptr){
            result.push_back(path);
            return result;
        }
        getTreePaths(root->left,path);
        getTreePaths(root->right,path);
        return result;
    }
private:
    vector<string> result;
    void getTreePaths(TreeNode *root,string path){
        if(root==nullptr){
            return;
        }
        path=path+"->"+to_string(root->val);
        if(root->left==nullptr&&root->right==nullptr){
            result.push_back(path);
        }
        getTreePaths(root->left,path);
        getTreePaths(root->right,path);
    }
};
```

# 404.左叶子的和

[题目链接](https://leetcode.cn/problems/sum-of-left-leaves/description/)

### 思路

方法一：递归法。在dfs的基础上做，传递一个参数用来标记当前节点是否是其父节点的左节点。

方法二：迭代法。与257中的迭代法思路基本一致。

### 时空复杂度

### 源码

方法一源码：

```
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        int sum=0;
        mySumOfLeftLeaves(root->left,sum,true);
        mySumOfLeftLeaves(root->right,sum,false);
        return sum;
    }
private:
    void mySumOfLeftLeaves(TreeNode *root,int &sum,bool isLeft){
        if(root==nullptr){
            return;
        }
        if(isLeft&&root->left==nullptr&&root->right==nullptr){
            sum+=root->val;
        }
        mySumOfLeftLeaves(root->left,sum,true);
        mySumOfLeftLeaves(root->right,sum,false);
    }
};
```

方法二源码：

```
class Solution {
public:
    int sumOfLeftLeaves(TreeNode *root) {
        int result = 0;

        std::stack<TreeNode *> nodeStack;
        TreeNode *lastVisited = nullptr;
        TreeNode *p = root;

        while (p || !nodeStack.empty()) {
            if (p) {
                nodeStack.push(p);
                p = p->left;
            } else {
                TreeNode *peekNode = nodeStack.top();
                if (peekNode->right && lastVisited != peekNode->right) {
                    // 如果右子树存在且未访问过，遍历右子树
                    p = peekNode->right;
                } else {
                    // 否则，访问当前节点
                    lastVisited = nodeStack.top();
                    nodeStack.pop();
                    if (!nodeStack.empty() && nodeStack.top()->left == peekNode && peekNode->left == nullptr &&
                        peekNode->right ==
                        nullptr) {
                        result += peekNode->val;
                    }
                }
            }
        }
        return result;
    }
};
```

