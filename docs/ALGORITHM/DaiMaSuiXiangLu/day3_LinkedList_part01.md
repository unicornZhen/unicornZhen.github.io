# 203.移除链表元素
[题目链接](https://leetcode.cn/problems/remove-linked-list-elements/)

### 思路

> 利用头节点简化代码操作，不要节省变量的使用

### 时空复杂度
> O(n):对链表扫描一遍。
>
> S(1)：头节点和几个变量，常数空间。

### 源码
```C++
/**  
 * Definition for singly-linked list. * struct ListNode { *     int val; *     ListNode *next; *     ListNode() : val(0), next(nullptr) {} *     ListNode(int x) : val(x), next(nullptr) {} *     ListNode(int x, ListNode *next) : val(x), next(next) {} * }; */class Solution {  
public:  
    ListNode* removeElements(ListNode* head, int val) {  
        if(head==nullptr)return head;  
        ListNode *pre_head=new ListNode(-1,head);  
        ListNode *pre=pre_head;  
        ListNode *cur=head;  
        ListNode *tmp;  
        while(cur!=nullptr){  
            if(cur->val!=val){  
                pre=cur;  
                cur=cur->next;  
            }else{  
                tmp=cur;  
                cur=cur->next;  
                pre->next=cur;  
                delete tmp;  
            }  
        }  
        return pre_head->next;  
    }  
};
```

# 707.设计链表
[题目链接](https://leetcode.cn/problems/design-linked-list/description/)

### 思路

> 按照标准的链表设计，除了头节点以外可以适当加一些链表长度、尾节点等简化或者加快链表的操作，在本题中加了链表长度来加速在指定位置删除或增加元素。

### 时空复杂度
> 就是标准单链表的时间空间复杂度分析方法

### 源码
```C++
class MyLinkedList {  
    typedef struct ListNode {  
        int val;  
        ListNode *next;  
  
        ListNode() {  
            val = 0;  
            next = nullptr;  
        }  
  
        ListNode(int val, ListNode *next) {  
            this->val = val;  
            this->next = next;  
        }  
    } ListNode;  
private:  
    ListNode *head;// the head of the MyLinkedList  
    int length;// the length of the MyLinkedList  
  
public:  
    MyLinkedList() {  
        head = new ListNode();  
        length = 0;  
    }  
  
    int get(int index) {  
        int i = 0;  
        if (index < 0)return -1;  
  
        ListNode *p = head;  
        while (p != nullptr && i <= index) {  
            p = p->next;  
            i++;  
        }  
        if (p == nullptr)return -1;  
        return p->val;  
    }  
  
    void addAtHead(int val) {  
        ListNode *p = new ListNode(val, head->next);  
        head->next = p;  
        length++;  
    }  
  
    void addAtTail(int val) {  
        ListNode *node = new ListNode(val, nullptr);  
        ListNode *p = head;  
        while (p->next != nullptr) {  
            p = p->next;  
        }  
        p->next = node;  
        length++;  
    }  
  
    void addAtIndex(int index, int val) {  
        if (index < 0 || index > length)return;  
  
        int i = 0;  
        ListNode *p = head;  
        while (i < index) {  
            p = p->next;  
            i++;  
        }  
        ListNode *node = new ListNode(val, p->next);  
        p->next = node;  
        length++;  
    }  
  
    void deleteAtIndex(int index) {  
        if (index < 0 || index >= length)return;  
  
        int i = 0;  
        ListNode *p = head;  
        while (i < index) {  
            p = p->next;  
            i++;  
        }  
        ListNode *tmp = p->next;  
        p->next = tmp->next;  
        delete tmp;  
        length--;  
    }  
};
```

# 206.反转链表
[题目链接](https://leetcode.cn/problems/reverse-linked-list/description/)

### 思路

> 利用头插法和尾插法的特点，将链表节点依次使用头插法再次插入从而逆转原链表,并使用头节点简化操作

### 时空复杂度
> O(n),S(1)

### 源码
```C++
class Solution {  
public:  
    ListNode* reverseList(ListNode* head) {  
        ListNode *pre_head=new ListNode();  
        ListNode *p=head;  
        while(head!=nullptr){  
            p=head;  
            head=head->next;  
            p->next=pre_head->next;  
            pre_head->next=p;  
        }  
        return pre_head->next;  
    }  
};
```