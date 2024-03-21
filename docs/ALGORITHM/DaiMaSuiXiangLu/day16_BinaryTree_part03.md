## 104.二叉树的最大深度

[题目链接](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)
### 思路
> 方法一：递归法，思想简单，不再赘述。
> 方法二：迭代法，使用队列遍历的方式得到深度。（这种关于二叉树题目使用迭代的方法是考虑从二叉树的三种迭代遍历方式、层序遍历的中寻找突破口）。应该采用后序遍历的迭代遍历方式，这样空间复杂度为O(h)。
> 本题目采用迭代法解答。

### 时空复杂度
> O(n)
> S(n)

### 源码
```C++
class Solution {  
public:  
    int maxDepth(TreeNode* root) {  
        if(root==nullptr){  
            return 0;  
        }  
        int result=0;  
        int cur_left_nodes=1;  
        int next_level_nodes=0;  
        queue<TreeNode *> que;  
        TreeNode *p;  
  
        que.push(root);  
        while(!que.empty()){  
            p=que.front();  
            que.pop();  
            cur_left_nodes--;  
            if(p->left!=nullptr){  
                que.push(p->left);  
                next_level_nodes++;  
            }  
            if(p->right!=nullptr){  
                que.push(p->right);  
                next_level_nodes++;  
            }  
            if(cur_left_nodes<=0){  
                cur_left_nodes=next_level_nodes;  
                next_level_nodes=0;  
                result++;  
            }  
        }  
        return result;  
    }  
};
```

## 111. 二叉树的最小深度

[题目链接](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/)
### 思路
> 方法一：递归法。注意有个坑，并不是最简单的返回最小值。
> 方法二：迭代法。与104中的迭代法基本无差别

### 时空复杂度
> O(n)、S(n)

### 源码
```C++
class Solution {  
public:  
    int minDepth(TreeNode* root) {  
        if(root==nullptr){  
            return 0;  
        }  
        int result=0;  
        int cur_left_nodes=1;  
        int next_level_nodes=0;  
        queue<TreeNode *> que;  
        TreeNode *p=root;  
  
        que.push(root);  
        while(!que.empty()){  
            p=que.front();  
            que.pop();  
            cur_left_nodes--;  
            if(p->left!=nullptr){  
                que.push(p->left);  
                next_level_nodes++;  
            }  
            if(p->right!=nullptr){  
                que.push(p->right);  
                next_level_nodes++;  
            }  
            if(p->right==nullptr&&p->left==nullptr){  
                return result+1;  
            }  
            if(cur_left_nodes<=0){  
                cur_left_nodes=next_level_nodes;  
                next_level_nodes=0;  
                result++;  
            }  
        }  
        return result;  
    }  
};
```

## 222.计算完全二叉树的节点个数

[题目链接](https://leetcode.cn/problems/count-complete-tree-nodes/description/)

### 思路
> 使用二分查找得到最后一层节点的个数，利用以下的关系：  
> 设k为最后一层节点的个数，k-1的二进制表示可以（1为向右，0向左，从高位到低位依次映射）映射为从根节点到最后一层最后节点的路径

### 时空复杂度
> $$(logn)^2$$：最后一层的二分查找的次数为logn，每次二分查找需要的操作次数为logn
> S(1)

### 源码
```C++
public:  
    int countNodes(TreeNode* root) {  
        if(root==nullptr){  
            return 0;  
        }  
        // get the height of the tree  
        int height=-1;  
        TreeNode *p=root;  
        while (p!= nullptr){  
            height++;  
            p=p->left;  
        }  
        int result=(int)pow(2,height)-1;  
        // 使用二分查找得到最后一层节点的个数，利用以下的关系：  
        // 设k为最后一层节点的个数，k-1的二进制表示可以（1为向右，0向左，从高位到低位依次映射）映射为从根节点到最后一层最后节点的路径  
        int start=0;  
        int end=(int)pow(2,height)-1;  
        int middle=0;  
        while (end>=start){  
            middle=start+(end-start)/2;  
            if(isExistNode(root,height,middle)){  
                if(!isExistNode(root,height,middle+1)){  
                    return result+middle+1;  
                }else{  
                    start=middle+1;  
                }  
            }else{  
                end=middle-1;  
            }  
        }  
        return result;  
    }  
private:  
    bool isExistNode(TreeNode *root,int height,int path){  
        if(path>=pow(2,height)||root==nullptr){  
            return false;  
        }  
        TreeNode *p=root;  
        for(int i=0;i<height;i++){  
            if((path>>(height-i-1))&1){  
                p=p->right;  
            }else{  
                p=p->left;  
            }  
        }  
        return p!= nullptr;  
    }

```

# 总结
* 针对二叉树（更甚至于图）这些由递归方法转为迭代方法解决，一般借鉴和分析二叉树的迭代遍历和层序遍历来解决问题
* 完全二叉树的节点计算的比较典型的思想、还有利用BFS计算二叉树的深度。