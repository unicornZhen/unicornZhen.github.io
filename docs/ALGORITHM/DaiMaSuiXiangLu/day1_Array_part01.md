# 704.二分查找
### 思路
很简单，就最基本的二分查找
### 时空复杂度
O(log n),S(1)

### 源码
```
class Solution {
public:
	int search(vector<int>& nums, int target) {
		int start=0;
		int end=nums.size()-1;
		int middle;
		while(start<=end){
			middle=(start+end)/2;
			if(nums[middle]<target){
			start=middle+1;
			}else if(nums[middle]==target){
			return middle;
			}else{
			end=middle-1;
			}
		}
		return -1;
	}
};
```

# 27.移除元素

### 思路
双指针法
### 时空复杂度
O(n),S(1)

### 源码
```
class Solution {
public:
	int removeElement(vector<int>& nums, int val) {
		int p_start=0;
		int p_end=nums.size()-1;
		int total=0;
		while(p_start<=p_end){
			if(nums[p_start]==val){
			// find the elements in nums not equal to val with p_end
				if(nums[p_end]!=val){
					nums[p_start]=nums[p_end];
				}
				p_end--;
			}else{
			total++;
			p_start++;
			}
		}
		return total;
	}

};
```
