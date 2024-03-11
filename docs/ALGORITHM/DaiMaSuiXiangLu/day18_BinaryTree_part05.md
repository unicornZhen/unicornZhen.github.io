# 513.找到二叉树底部最左边的节点的值

[题目链接](https://leetcode.cn/problems/find-bottom-left-tree-value/description/)

### 思路

方法一：迭代法。利用层序遍历做，计算每层的开始，比较简单，与day16中的一些题目类似。

方法二：递归法。设每颗树要找的值为x。

i 递归。根的x=（左子树的高度>=右子树的高度）？左子树的x：右子树的x。

ii 边界条件。若根的左右子树均为空，则根的x=根的值。

具体见代码

### 时空复杂度

O(n)、S(n)

### 源码

```C++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        int result=0;
        myFindBottomLeftValue(root,result);
        return result;
    }
private:
    int myFindBottomLeftValue(TreeNode* root,int &value){
        if(root==nullptr){
            value=INT_MAX;
            return 0;
        }
        if(root->left==nullptr&&root->right==nullptr){
            value=root->val;
            return 1;
        }
        int r_value=0;
        int l_value=0;
        int l_height=myFindBottomLeftValue(root->left,l_value);
        int r_height=myFindBottomLeftValue(root->right,r_value);
        value=(l_height>=r_height)?l_value:r_value;
        return max(l_height,r_height)+1;
    }
};
```



# 113.路径和II（类似有112.路径和）

[题目链接](https://leetcode.cn/problems/path-sum-ii/description/)

### 思路

方法一：迭代法。二叉树的迭代后序遍历，与day17中的一些题目的解题思路一样。

方法二：递归法。

本题采用方法二解答。

### 时空复杂度

O(n)、S(n)

### 源码

```C++
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int>> result;
        if(root==nullptr){
            return result;
        }

        vector<int> path;
        findPath(root,result,targetSum,path);
        return result;
    }
private:
    void findPath(TreeNode *root,vector<vector<int>> &result,int sum,vector<int> &path){
        if(root==nullptr){
            return;
        }

        path.push_back(root->val);
        if(root->left==nullptr&&root->right==nullptr&&sum==root->val){
            result.push_back(path);
        }
        findPath(root->left,result,sum-root->val,path);
        findPath(root->right,result,sum-root->val,path);
        path.pop_back();
    }
};

```



# 106.根据后序遍历和中序遍历构造二叉树（同类的有：105.根据前序遍历和后序遍历构造二叉树）

[题目链接](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

### 思路

递归法，根据中序遍历左根右，后序遍历左右根，递归地自顶向下构造二叉树

### 时空复杂度

O(n)、S(n)

### 源码

```C++
class Solution {
public:
    TreeNode *buildTree(vector<int> &inorder, vector<int> &postorder) {
        return myBuildTree(inorder, postorder, postorder.size() - 1, 0, inorder.size() - 1);
    }

private:
    TreeNode *myBuildTree(vector<int> &inorder, vector<int> &postorder, int root_pos, int i_start, int i_end) {
        if (i_start > i_end) {
            return nullptr;
        }
        int root_pos_i = i_start;
        while (inorder[root_pos_i] != postorder[root_pos] && root_pos_i <= i_end) {
            root_pos_i++;
        }
        TreeNode *node = new TreeNode(postorder[root_pos]);
        node->left = myBuildTree(inorder, postorder, root_pos - i_end + root_pos_i - 1, i_start, root_pos_i - 1);
        node->right = myBuildTree(inorder, postorder, root_pos - 1, root_pos_i + 1, i_end);
        return node;
    }
};
```

