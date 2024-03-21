# 669.修剪二叉搜索树

[题目链接](https://leetcode.cn/problems/trim-a-binary-search-tree/)

### 思路

> 方法一：递归法。利用BST本身的性质修剪BST，详情见源码
>
> 方法二：迭代法。在此不再赘述，使用迭代法可以使得空间复杂度降为O(1)，思路与方法一中的递归类似
>

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(root==nullptr)return nullptr;
        if(root->val<low){
            return trimBST(root->right,low,high);
        }else if(root->val>high){
            return trimBST(root->left,low,high);
        }else{
            root->left=trimBST(root->left,low,high);
            root->right=trimBST(root->right,low,high);
            return root;
        }
    }
};
```



# 108.将有序数组转为AVL树

[题目链接](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)

### 思路

> 递归法。每次选取数组的中间元素作为根，以中间元素为划分，分别建立左子树和右子树。

### 时空复杂度

> $$O(n),S(logn)$$

### 源码

```C++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return mySortedArrayToBST(nums,0,nums.size()-1);
    }
private:
    TreeNode* mySortedArrayToBST(vector<int>& nums,int start,int end){
        if(start>end){
            return nullptr;
        }

        int middle=start+(end-start)/2;
        TreeNode* node=new TreeNode(nums[middle]);
        node->left=mySortedArrayToBST(nums,start,middle-1);
        node->right=mySortedArrayToBST(nums,middle+1,end);
        return node;
    }
};
```



# 538.将BST转为累加树

[题目链接](https://leetcode.cn/problems/convert-bst-to-greater-tree/description/)

### 思路

> 递归法。初步想法中序遍历BST，使用数组按照升序储存BST中的元素，再中序遍历，使用这个数组得到各个节点应该累加的值。仔细思考可以进一步优化，一遍遍历即可完成，只需要中序遍历时交换左右子树的遍历顺序即可按照降序次序访问二叉树的每个节点。关键是如何得到二叉树的降序遍历顺序。

### 时空复杂度

> O(n),S(n)

### 源码

```C++
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        int sum=0;
        myConverBST(root,sum);
        return root;
    }
private:
    void myConverBST(TreeNode* root,int &sum){
        if(root==nullptr){
            return;
        }

        myConverBST(root->right,sum);
        sum+=root->val;
        root->val=sum;
        myConverBST(root->left,sum);
    }
};
```

## 总结

* 通过交换左右子树的遍历顺序得到降序序列的二叉树
* 修剪BST的递归设计需要仔细体会
