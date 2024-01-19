# 28.字符串匹配
### 思路：
方法一：实现KMP算法（KMP算法比较难，懂得大致思路和KMP的应用就行，让gpt实现就行）
方法二：直接调用字符串的find方法
本题采用的是方法一，实现KMP算法
### 时空复杂度：
O(n+m)、S(m):n是text的长度，m是pattern的长度
### 源码：
```
class Solution {  
public:  
    void computeLPSArray(string pat, int M, vector<int>& lps) {  
        int len = 0;  
        lps[0] = 0;  
        int i = 1;  
        while (i < M) {  
            if (pat[i] == pat[len]) {  
                len++;  
                lps[i] = len;  
                i++;  
            } else {  
                if (len != 0) {  
                    len = lps[len - 1];  
                } else {  
                    lps[i] = 0;  
                    i++;  
                }  
            }  
        }  
    }  
  
    int strStr(string txt, string pat) {  
        int M = pat.size();  
        int N = txt.size();  
        if (M == 0) return 0;  
        vector<int> lps(M, 0);  
        computeLPSArray(pat, M, lps);  
        int i = 0;  
        int j = 0;  
        while (i < N) {  
            if (pat[j] == txt[i]) {  
                j++;  
                i++;  
            }  
            if (j == M) {  
                return i - j;  
                j = lps[j - 1];  
            } else if (i < N && pat[j] != txt[i]) {  
                if (j != 0)  
                    j = lps[j - 1];  
                else  
                    i = i + 1;  
            }  
        }  
        return -1;  
    }  
};
```

# 459.重复的子字符串
### 思路：
方法一：蛮力法，注意分析题目，减少不必要的遍历
方法二：(s+s).find(s,1)!=s.size,其中s为输入字符串，注意理解条件的充分性和必要性
方法三：使用KMP算法，但不懂如何使用
### 时空复杂度：
方法一：O(n2),S(1)
方法二：O(n),S(n)
方法三：O(n),S(n)
### 源码：
``` 
class Solution {  
public:  
    bool repeatedSubstringPattern(string s) {  
        return (s+s).find(s,1)!=s.size();  
    }  
};
```