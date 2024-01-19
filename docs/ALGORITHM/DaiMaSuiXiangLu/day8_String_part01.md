# 344.反转字符串
### 思路：
简单的前半部分的字符与后半部分的字符交换
### 时空复杂度：
O(n)、S(1)
### 源码：
```
class Solution {  
public:  
    void reverseString(vector<char>& s) {  
        for(int i=0;i<s.size()/2;i++){  
            swap(s[i],s[s.size()-1-i]);  
        }  
    }  
};
```

# 541.反转字符串II
### 思路：
本质上与最基础的反转字符串区别不大，注意迭代中swap交换的下标的控制就行
### 时空复杂度：
O(n):一趟扫描
S(1)：常数的空间，交换的需要的临时变量char和一些下标变量
### 源码：
```
class Solution {  
public:  
    string reverseStr(string s, int k) {  
        int i=0;  
        while(i+k<=s.size()){  
            for(int j=i;j<(i+k/2);j++){  
                swap(s[j],s[i+k-1-j+i]);  
            }  
            i+=2*k;  
        }  
  
        // reverse the left chars where the num of the left chars is smaller than k  
        if(i<s.size()){  
            for(int j=i;j<i+(s.size()-i)/2;j++){  
                swap(s[j],s[s.size()-1-j+i]);  
            }  
        }  
        return s;  
    }  
};
```

# 卡码网：54.替换数字
### 思路：
很简单，就拼接字符串
### 时空复杂度：
O(n)、S(n)
### 源码：
```
#include <iostream>

using namespace std;

int main(){
    string s;
    cin>>s;
    string result;
    const string tmp="number";
    for(auto it:s){
        if(it>='0'&&it<='9'){
            result+=tmp;
        }else{
            result+=it;
        }
    }
    cout<<result;
}
```

# 151.反转字符串里的单词
### 思路：
设要反转的字符串为s
方法一：
从后往前识别出s每个单词的开始位置和结束位置，依次将这些单词插入到结果中。
方法二：
先对s反转得到s1，再从头开始对s1中的每个单词单独反转即可得到最终结果
本次我采取的是方法一
### 时空复杂度：
O(n)：扫描两遍
S(1):除了输出数据占用的空间，其他只占用常数空间
### 源码：
```
class Solution {  
public:  
    string reverseWords(string s) {  
        string result;  
        const char split = ' ';  
        int start = 1;  
        int end = 0;  
        for (int i = s.size() - 1; i >= 0; i--) {  
            // find the end index of a new word  
            while (i >= 0 && s[i] == split) {  
                i--;  
            }  
            if (i < 0) {  
                break;  
            } else {  
                end = i;  
            }  
  
            // determine the start index of the same new word  
            // 注意单词位于字符串的开头  
            while (i >= 0 && s[i] != split) {  
                i--;  
            }  
  
            start=i+1;  
            if(!result.empty()){  
                result+=" ";  
            }  
            result += s.substr(start, end - start + 1);  
        }  
        return result;  
    }  
};
```

# 55.右旋转字符串
### 思路：
注意多次旋转的思想，与151中方法二类似
### 时空复杂度：
O(n)
S(1)
### 源码：
```
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

int main() {
    string str;
    int k=0;
    cin>>k;
    cin>>str;
    if(k>str.size())return 0;
    reverse(str.begin(), str.end());
    reverse(str.begin(), str.begin()+k);
    reverse(str.begin()+k,str.end());
    cout<<str;
    return 0;
}
```