# 子集

### 题目

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

```/
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```



### 题解

#### 解法一

递归 + 回溯。由于无重复数字，因此只需要对于每一位考虑是否选取即可。

子集和全排列的区别在于：

+ 子集的关键在于是否选取一个元素，而不关心元素位置；
+ 全排列必须选所有元素，且需要考虑元素位置；

（自己做题的时候刚开始没想到，一直想着全排列的做法，该把哪个数放到第一个...真的是脑子僵化了）

```c++
// 执行用时 :8 ms, 在所有 C++ 提交中击败了82.08%的用户
// 内存消耗 :12.8 MB, 在所有 C++ 提交中击败了9.16%的用户

class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        if(nums.size() == 0) 
            return {{}};

        vector<vector<int>> res;
        vector<int> temp;
        helper(nums, res, temp, 0);
        return res;
    }

    void helper(vector<int>& nums, vector<vector<int>>& res, vector<int>& temp, int index){
        if(index == nums.size()){
            res.push_back(temp);
            return;
        }

        for(int i = 0; i < 2; i++){
            if(i == 0){
                helper(nums, res, temp, index + 1);
            }else{
                temp.push_back(nums[index]);
                helper(nums, res, temp, index + 1);
                temp.pop_back();
            }
        }
    }
};
```



#### 解法二

迭代法。利用子集的特点，n个不重复元素集合的子集个数是2的n次方，也就是每多增加一个元素，子集个数也就翻倍。那么，我们就可以顺序遍历原始数组，每一次将上一步的全部自己复制一份并都加入新的数。

```c++
// 执行用时 :4 ms, 在所有 C++ 提交中击败了99.55%的用户
// 内存消耗 :8.8 MB, 在所有 C++ 提交中击败了96.19%的用户

class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {

        vector<vector<int>> res;
        res.push_back({});
        for(int i = 0; i < nums.size(); i++){
            int size = res.size();
            for(int j = 0; j < size; j++){
                vector<int> temp = res[j];
                temp.push_back(nums[i]);
                res.push_back(temp);
            }
        }
        return res;
    }
};
```



#### 解法三

位运算，待完成...

