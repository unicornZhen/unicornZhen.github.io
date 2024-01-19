# 977.有序数组的平方
### 思路：
双指针法，归并排序
### 时空复杂度：
O(n)
S(n)：原有题目结果返回需要的复杂度，可以利用O(1)的归并排序方法

### 源码
```
class Solution {  
public:  
    vector<int> sortedSquares(vector<int>& nums) {  
        int middle=0;  
        vector<int> result;  
        // get the the index of the minmum of fabs(nums[index])  
        for(int i=0;i<nums.size()-1;i++){  
            if(fabs(nums[i])>fabs(nums[i+1])){  
                middle=i+1;  
            }  
        }  
  
        // merge sort to (0,end],[start,nums.size())  
        int end=middle;  
        int start=middle+1;  
        while(end>=0&&start<nums.size()){  
            if(fabs(nums[end])<fabs(nums[start])){  
                result.push_back(nums[end]);  
                end--;  
            }else{  
                result.push_back(nums[start]);  
                start++;  
            }  
        }  
        while(end>=0){  
            result.push_back(nums[end]);  
            end--;  
        }  
        while(start<nums.size()){  
            result.push_back(nums[start]);  
            start++;  
        }  
  
        for(auto &it:result){  
            it=it*it;  
        }  
        return result;  
    }  
};
```
note:可以进一步简化，从结果的最大值开始排序或者最小值开始排序也行

# 209.最小尺寸的字数组和
### 思路：
滑动窗口或者先预处理（排序）再找最小尺寸的数组，这里以滑动窗口为例
### 时空复杂度：
O(n),S(1)

### 源码
```
class Solution {  
public:  
    int minSubArrayLen(int target, vector<int>& nums) {  
        int start=0;  
        int end=0;  
        int sum=nums[start];  
        int result=0;  
        // get the first sliding window  
        while(sum<target){  
            end++;  
            if(end>=nums.size()){  
                return result;  
            }  
            sum+=nums[end];  
        }  
        result=end-start+1;  
  
        // get the minSubArrayLen  
        while(start<nums.size()&&end<nums.size()){  
            sum-=nums[start];  
            start++;  
            while(sum<target){  
                end++;  
                if(end>=nums.size()){  
                    return result;  
                }  
                sum+=nums[end];  
            }  
            result=end-start+1>result?result:(end-start+1);  
        }  
        return result;  
    }  
};
```