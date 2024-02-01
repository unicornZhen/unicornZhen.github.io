# 235.BST的最近公共祖先

[题目链接](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

### 思路

> 方法一：按照二叉树的最近公共祖的方法做

> 方法二：利用BST的性质，可以分别两个节点得到两条路径，根据得到的路径再找到最近公共祖先。可以进一步得到优化，同时寻找两个节点，寻找的过程就可以得到最近公共祖先，具体见下面的源码

本题采用方法二。

### 时空复杂度

> 方法一：O(n),S(n)

> 方法二：O(logn)，使用迭代法解决的话为S(1)

### 源码

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode *result=root;
        TreeNode *node=root;
        while(result!=nullptr){
            if(result->val>p->val&&result->val>q->val){
                result=result->left;
            }else if(result->val<p->val&&result->val<q->val){
                result=result->right;
            }else{
                return  result;
            }
        }
        return result;
    }
};
```



# 701.BST的插入

[题目链接](https://leetcode.cn/problems/insert-into-a-binary-search-tree/description/)

### 思路

> 很简单的BST的节点插入

### 时空复杂度

> O(logn),S(1)

### 源码

```
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode *node=new TreeNode(val);
        if(root==nullptr)return node;

        TreeNode *pre=root;
        TreeNode *p=root;
        while(p!=nullptr){
            pre=p;
            if(p->val<val){
                p=p->right;
            }else {
                p=p->left;
            }
        }

        if(pre->val<val){
            pre->right=node;
        }else{
            pre->left=node;
        }
        return root;
    }
};
```

# 450.BST中节点的删除

[题目链接](https://leetcode.cn/problems/delete-node-in-a-bst/description/)

### 思路

使用chatgpt解答，算作基础知识，不需要自己记，只需要会应用就行，理解大致的思想和注意递归设计的巧妙

### 时空复杂度

O(logn),S(logn)

### 源码

```
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        // 分析:删除根；删除叶子节点；删除有左孩子的节点；删除有右孩子的节点
        if (root == nullptr) {
            return root;
        }

        if (key < root->val) {
            root->left = deleteNode(root->left, key);
        } else if (key > root->val) {
            root->right = deleteNode(root->right, key);
        } else {
            // 当前节点就是要删除的节点

            // 情况1: 没有子节点或只有一个子节点
            if (root->left == nullptr) {
                TreeNode* temp = root->right;
                delete root;
                return temp;
            } else if (root->right == nullptr) {
                TreeNode* temp = root->left;
                delete root;
                return temp;
            }

            // 情况2: 有两个子节点
            TreeNode* temp = findMin(root->right); // 找到右子树的最小节点
            root->val = temp->val; // 将当前节点的值替换为右子树最小节点的值
            root->right = deleteNode(root->right, temp->val); // 删除右子树最小节点
        }

        return root;
    }
private:
    TreeNode* findMin(TreeNode* node) {
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }
};
```

