# 删除排序数组中的重复项

### 题目

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

**示例 1:**

```
给定 nums = [1,1,1,2,2,3],

函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。

你不需要考虑数组中超出新长度后面的元素。
```

**示例 2:**

```
给定 nums = [0,0,1,1,1,1,2,3,3],

函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3 。

你不需要考虑数组中超出新长度后面的元素。
```



### 题解

#### 解法一

直接使用 vector STL 中的erase函数。

```c++
// 执行用时 :12 ms, 在所有 C++ 提交中击败了97.34%的用户
// 内存消耗 :8.6 MB, 在所有 C++ 提交中击败了92.46%的用户

class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() <= 2){
            return nums.size();
        }
        int count = 1;
        vector<int>::iterator it = nums.begin() + 1;
        while(it != nums.end()){
            if(*it == *(it - 1)){
                count++;
                if(count > 2)
                    it = nums.erase(it);
                else{
                    it++;
                }
            }else{
                count = 1;
                it++;
            }
        }    
        return nums.size();
    }
};
```



#### 解法二

两个指针：前一个指针用于遍历，后一个指针指向要覆盖的元素。

例如，数组`[1,1,1,2,2,3]`，我们要覆盖数组中的第三个1。那么当快指针遍历发现出现了三个元素时，慢指针停止移动直到快指针找到下一个不为1的元素，并将这个元素覆盖掉第三个1，然后慢指针指向下一个元素。

```c++
// 执行用时 :16 ms, 在所有 C++ 提交中击败了74.09%的用户
// 内存消耗 :8.8 MB, 在所有 C++ 提交中击败了74.02%的用户

class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() <= 2){
            return nums.size();
        }

        int j = 1, c = 1;
        for(int i = 1; i < nums.size(); i++){
            if(nums[i] == nums[i - 1]){
                c++;
                if(c <= 2){
                    nums[j] = nums[i];
                    j++;
                }
            }else{
                c = 1;
                nums[j] = nums[i];
                j++;
            }
        }
        return j;
    }
};
```



