# 24 两两交换链表中的节点
[题目链接](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

### 思路

> 简单的遍历链表，两个两个的遍历，每两个互相交换，使用节点指针记住链表的位置信息，这个很重要，防止断链。

### 时空复杂度
> O(n),S(1)

### 源码
```C++
class Solution {  
public:  
    ListNode* swapPairs(ListNode* head) {  
        ListNode* pre_head=new ListNode(0,head);  
        if(head==nullptr)return pre_head->next;  
        ListNode* pre_first=pre_head;  
        ListNode* first=head;  
        ListNode* second=head->next;  
        ListNode* tmp;  
        while(second!=nullptr){  
            tmp=second->next;  
            pre_first->next=second;  
            second->next=first;  
            first->next=tmp;  
  
            // update the value of pre_first,first,second  
            pre_first=first;  
            first=tmp;  
            if(first==nullptr){  
                second=nullptr;  
            }else{  
                second=first->next;  
            }  
        }  
        return pre_head->next;  
    }  
};
```


# 19.移除链表的最后第n个节点
[题目链接](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)

### 思路

> 一趟遍历，使用双指针找到目标节点的前驱节点，理清快指针需要领先慢指针多少。

### 时空复杂度
> O(n)：一趟扫描
> S(1)

### 源码
```C++
class Solution {  
public:  
    ListNode* removeNthFromEnd(ListNode* head, int n) {  
        ListNode *pre_head=new ListNode(0,head);  
        ListNode *p1=pre_head;  
        // point to the precursor node of the target node  
        ListNode *p2=pre_head;  
        for(int i=0;i<=n;i++){  
            p1=p1->next;  
        }  
        while(p1!=nullptr){  
            p1=p1->next;  
            p2=p2->next;  
        }  
        p1=p2->next;  
        p2->next=p1->next;  
        delete p1;  
        return pre_head->next;  
    }  
};
```

# 面试题 02.07.链表相交
[题目链接](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

### 思路

> 使用快慢指针在O(n+m)时间复杂度内找到公共的起始节点，先计算的出链表A和B
> 的长度，依次长度设置快慢指针，再比较快慢指针是否相等找到起始节点。

### 时空复杂度
> O(n+m)：分别扫描两个链表各两趟
> S(1)：

### 源码
```C++
class Solution {  
public:  
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {  
        int len_a=0;  
        int len_b=0;  
        ListNode *p_a=headA;  
        ListNode *p_b=headB;  
        while(p_a!=nullptr){  
            len_a++;  
            p_a=p_a->next;  
        }  
        while(p_b!=nullptr){  
            len_b++;  
            p_b=p_b->next;  
        }  
  
        p_a=headA;  
        p_b=headB;  
        if(len_a>len_b){  
            for(int i=0;i<len_a-len_b;i++){  
                p_a=p_a->next;  
            }  
        }else{  
            for(int i=0;i<len_b-len_a;i++){  
                p_b=p_b->next;  
            }  
        }  
  
        while(p_a!=nullptr&&p_b!=nullptr){  
            if(p_a==p_b)return p_a;  
            p_a=p_a->next;  
            p_b=p_b->next;  
        }  
        return nullptr;  
    }  
};
```

# 142 环形链表 II
[题目链接](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

### 思路

> <span style="color:red">当时没有做出来，设置快慢指针，利用数学推理，得到环的入口节点</span>

### 时空复杂度
> O(n)
> S(1)

### 源码
```C++
class Solution {  
public:  
    ListNode *detectCycle(ListNode *head) {  
        ListNode* pre_head=new ListNode(0,head);  
        ListNode *fast=pre_head;  
        ListNode *slow=pre_head;  
        while(fast!=nullptr&&slow!=nullptr){  
            slow=slow->next;  
            fast=fast->next;  
            if(fast==nullptr)return nullptr;  
            fast=fast->next;  
            if(fast==slow)break;  
        }  
        if(fast!=slow)return nullptr;  
        // 必然会存在环  
        fast=pre_head;  
        while(fast!=slow){  
            fast=fast->next;  
            slow=slow->next;  
        }  
        return slow;  
    }  
};
```