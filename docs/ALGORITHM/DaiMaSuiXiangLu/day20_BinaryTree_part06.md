# 654.最大二叉树

[题目链接](https://leetcode.cn/problems/maximum-binary-tree/description/)

### 思路

> 方法一：按照题目所说的采用递归方法。
>
> 方法二：使用单调栈。要构建两个分别用于构建左子树和右子树的单调栈。
>
> 本人虽然使用的方法一解答题目，但由于方法一过于简单，在这里只介绍方法二,注意方法二目前还未去详细理解。

### 时空复杂度

> O(n)、S(n)

### 源码

```C++
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        int n = nums.size();
        vector<int> stk;
        vector<int> left(n, -1), right(n, -1);
        vector<TreeNode*> tree(n);
        for (int i = 0; i < n; ++i) {
            tree[i] = new TreeNode(nums[i]);
            while (!stk.empty() && nums[i] > nums[stk.back()]) {
                right[stk.back()] = i;
                stk.pop_back();
            }
            if (!stk.empty()) {
                left[i] = stk.back();
            }
            stk.push_back(i);
        }

        TreeNode* root = nullptr;
        for (int i = 0; i < n; ++i) {
            if (left[i] == -1 && right[i] == -1) {
                root = tree[i];
            }
            else if (right[i] == -1 || (left[i] != -1 && nums[left[i]] < nums[right[i]])) {
                tree[left[i]]->right = tree[i];
            }
            else {
                tree[right[i]]->left = tree[i];
            }
        }
        return root;
    }
};

```



# 617.合并二叉树

[题目链接](https://leetcode.cn/problems/merge-two-binary-trees/description/)

### 思路

> 方法一：递归。按照题目采用dfs，从上到下合并二叉树各个节点。

> 方法二：层序遍历。也是从上到下合并各个二叉树节点。

本题采用方法一。

### 时空复杂度

> 两种方法的时空复杂度都为O(n),S(n)

### 源码

```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1==nullptr){
            return root2;
        }
        if(root2==nullptr){
            return root1;
        }
        TreeNode* left=root2->left;
        TreeNode* right=root2->right;
        root1->val+=root2->val;
        root1->left=mergeTrees(root1->left,root2->left);
        root1->right=mergeTrees(root1->right,root2->right);
        delete root2;
        return root1;
    }
};
```



# 700.在BST中搜索

[题目链接](https://leetcode.cn/problems/search-in-a-binary-search-tree/description/)

### 思路

> 很简单，就BST的二分搜索方法

### 时空复杂度

> logn

### 源码

```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        TreeNode* p=root;
        while(p!=nullptr){
            if(p->val==val){
                return p;
            }else if(p->val<val){
                p=p->right;
            }else{
                p=p->left;
            }
        }
        return nullptr;
    }
};
```



# 98.BST的判定

[题目链接](https://leetcode.cn/problems/validate-binary-search-tree/description/)

### 思路

> 采用二叉树的迭代中序遍历方式，记录当前遍历值的前一个值来判定当前节点是否合法，若所有节点都合法，则此二叉树是BST

### 时空复杂度

>  O(n),s(n)

### 源码

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> nodeStack;
        TreeNode *p=root;
        long long pre_val=INT_MIN;
        pre_val--;
        while (p || !nodeStack.empty()) {
            while (p) {
                // 将左子树节点入栈
                nodeStack.push(p);
                p = p->left;
            }

            // 出栈访问节点
            p = nodeStack.top();
            nodeStack.pop();
            if(p->val<=pre_val){
                return false;
            }
            pre_val=p->val;

            // 访问右子树
            p = p->right;
        }
        return true;
    }
};
```

