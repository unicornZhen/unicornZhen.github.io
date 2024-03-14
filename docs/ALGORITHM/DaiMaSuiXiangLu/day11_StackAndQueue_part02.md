## 20.有效的括号

[20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/description/)

### 思路

> 栈的典型应用

### 时空复杂度
> n为字符串的长度
> O(n)
> S(n)

### 源码

```
class Solution {  
public:  
    bool isValid(string s) {  
        stack<char> brackets;  
        for(auto it:s){  
            switch (it) {  
                case '(':  
                    brackets.push(it);  
                    break;  
                case '{':  
                    brackets.push(it);  
                    break;  
                case '[':  
                    brackets.push(it);  
                    break;  
                case ')':  
                    if(brackets.empty()||brackets.top()!='('){  
                        return false;  
                    }  
                    brackets.pop();  
                    break;  
                case '}':  
                    if(brackets.empty()||brackets.top()!='{'){  
                        return false;  
                    }  
                    brackets.pop();  
                    break;  
                case ']':  
                    if(brackets.empty()||brackets.top()!='['){  
                        return false;  
                    }  
                    brackets.pop();  
                    break;  
                default:  
                    return false;  
  
            }  
        }  
        if(!brackets.empty()){  
            return false;  
        }  
        return true;  
    }  
};
```

## 1047.移除所有相邻的重复字符

[1047. 删除字符串中的所有相邻重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)

### 思路

> 本质上与20.有效的括号的解题思路一致，按照题目要求模拟做的话会发现与20中异曲同工，也是栈的应用。

### 时空复杂度
> n为字符串长度
> O(n)：扫描、栈倒入结果、结果中的字符反转
> S(n)：栈复杂度为O(n)，结果复杂度为O(n)

### 源码

```C++
class Solution {  
public:  
    string removeDuplicates(string s) {  
        stack<char> st;  
        string result;  
        for(auto it:s){  
            if (!st.empty()&&st.top()==it){  
                st.pop();  
            }else{  
                st.push(it);  
            }  
        }  
        while (!st.empty()){  
            result.push_back(st.top());  
            st.pop();  
        }  
        reverse(result.begin(), result.end());  
        return result;  
    }  
};
```

## 150.计算逆波兰表达式

[150. 逆波兰表达式求值 - 力扣（LeetCode）](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)

### 逆波兰表达式

> 逆波兰表达式（Reverse Polish Notation，RPN），也称为后缀表达式，是一种数学表达式的表示方式，其中运算符在操作数之后。这种表示法消除了括号的需求，同时确保了表达式的唯一解释。
>
> 例如，中缀表达式 "3 + 4 * 5" 可以转换为逆波兰表达式 "3 4 5 * +"。在逆波兰表达式中，操作数直接跟在操作符后面，而不需要括号来指定运算次序。
>
> 逆波兰表达式的计算通常使用栈来实现。遍历表达式，当遇到操作数时，将其入栈；当遇到操作符时，弹出栈顶的两个操作数进行计算，并将结果重新入栈。这样，遍历完整个表达式后，栈中的元素就是最终的计算结果。

### 思路
> 用栈实现逆波兰表达式的计算，思路比较简单，见上述的逆波兰表达式简介，需要注意的是switch中条件的设置，当时写的时候忽略了"-12"和"-"

### 时空复杂度
> O(n)
> S(n)

### 源码

```C++
class Solution {  
public:  
    int evalRPN(vector<string> &tokens) {  
        stack<int> operands;  
        int op1=0;  
        int op2=0;  
        // convert an integer of type string to an integer of type int  
        for (auto &it: tokens) {  
            switch (it[0]) {  
                case '+':  
                    // handle the number like：+12  
                    if(it.size()==1) {  
                        op2 = operands.top();  
                        operands.pop();  
                        op1 = operands.top();  
                        operands.pop();  
                        operands.push(op1 + op2);  
                    }else{  
                        operands.push(stoi(it));  
                    }  
                    break;  
                case '-':  
                    // handle negative number:-12  
                    if(it.size()==1){  
                        op2=operands.top();  
                        operands.pop();  
                        op1=operands.top();  
                        operands.pop();  
                        operands.push(op1-op2);  
                    }else{  
                        operands.push(stoi(it));  
                    }  
                    break;  
                case '*':  
                    op2=operands.top();  
                    operands.pop();  
                    op1=operands.top();  
                    operands.pop();  
                    operands.push(op1*op2);  
                    break;  
                case '/':  
                    op2=operands.top();  
                    operands.pop();  
                    op1=operands.top();  
                    operands.pop();  
                    operands.push(op1/op2);  
                    break;  
  
                    // handle the digits  
                default:  
                    operands.push(stoi(it));  
                    break;  
            }  
        }  
        return operands.top();  
    }  
};
```

# 总结
* 栈的典型应用（括号匹配），还有栈的扩展应用，总之栈是十分强大的
* 逆波兰表达式的结果计算与如何从中缀表达式转换得到逆波兰表达式（后缀表达式）
* 对于C++的string，a+=b的效率比a=a+b的效率要高，因为a+=b可以直接在a的末尾追加b，而不需要创建一个新的string对象来存储a和b的和。而a=a+b则需要创建一个临时的string对象来保存a和b的和，然后再赋值给a。这样就会增加内存分配和拷贝的开销。从这里可以看出智能指针的作用