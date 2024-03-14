## 232.用栈实现队列

[232. 用栈实现队列 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-queue-using-stacks/description/)

### 思路

> 两个栈s1,s2实现队列，s1倒入到s2中再弹出，从而满足队列的先进先出。注意一些优化的小技巧，pop之后不立马把s2中的内容倒入s1中。

### 时空复杂度

> 分析各项操作的时间复杂度较为简单，在此不赘述。

### 源码
```C++
class MyQueue {  
public:  
    MyQueue() {  
  
    }  
  
    void push(int x) {  
        while (!s2.empty()){  
            s1.push(s2.top());  
            s2.pop();  
        }  
        s1.push(x);  
    }  
  
    int pop() {  
        if (s1.empty()&&s2.empty())return -1;  
        while (!s1.empty()){  
            s2.push(s1.top());  
            s1.pop();  
        }  
        int value=s2.top();  
        s2.pop();  
        return value;  
    }  
  
    int peek() {  
        if(s1.empty()&&s2.empty())return -1;  
        if(!s2.empty()){  
            return s2.top();  
        }  
        while (!s1.empty()){  
            s2.push(s1.top());  
            s1.pop();  
        }  
        return s2.top();  
    }  
  
    bool empty() {  
        return s1.empty()&&s2.empty();  
    }  
  
private:  
    stack<int> s1, s2;  
};
```

## 225.用队列实现栈

[225. 用队列实现栈 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-stack-using-queues/description/)

### 思路

> 题解中方法：本质上使得队列中的元素的顺序是一个栈，所以需要做入栈时的维护操作，其他操作都是O(1)，方法上比我的方法要好一些。
> **题解用一个队列实现栈的源码**

```C++
class MyStack {  
public:  
    queue<int> q;  
  
    /** Initialize your data structure here. */  
    MyStack() {  
  
    }  
  
    /** Push element x onto stack. */  
    void push(int x) {  
        int n = q.size();  
        q.push(x);  
        for (int i = 0; i < n; i++) {  
            q.push(q.front());  
            q.pop();  
        }  
    }  
  
    /** Removes the element on top of the stack and returns that element. */  
    int pop() {  
        int r = q.front();  
        q.pop();  
        return r;  
    }  
  
    /** Get the top element. */  
    int top() {  
        int r = q.front();  
        return r;  
    }  
  
    /** Returns whether the stack is empty. */  
    bool empty() {  
        return q.empty();  
    }  
};
```
> 我采用的方法是pop时和top时取得队列的首个元素，需要对pop和top进行一些操作，push复杂度为O(1)，pop和top复杂度为O(n)

### 时空复杂度
> 时间复杂度：push复杂度为O(1)，pop和top复杂度为O(n)
> S(n)：使用一个队列

### 源码
```C++
class MyStack {  
public:  
    MyStack() {  
  
    }  
  
    void push(int x) {  
        que.push(x);  
    }  
  
    int pop() {  
        if(que.empty())return -1;  
  
        int size=que.size();  
        for(int i=0;i<size-1;i++){  
            que.push(que.front());  
            que.pop();  
        }  
        int value=que.front();  
        que.pop();  
        size--;  
        return value;  
    }  
  
    int top() {  
        if(que.empty())return -1;  
  
        int value=0;  
        int size=que.size();  
        for(int i=0;i<size;i++){  
            if(i==size-1){  
                value=que.front();  
            }  
            que.push(que.front());  
            que.pop();  
        }  
        return value;  
    }  
  
    bool empty() {  
        return que.empty();  
    }  
private:  
    queue<int> que;  
};
```
