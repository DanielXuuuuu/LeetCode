# 包含重复数字的全排列

### 题目

给定一个可包含重复数字的序列，返回所有不重复的全排列。

示例:

```/
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```



### 题解

延续最基本的寻找全排列思路，即使用递归从左到右依次确定下每一位要放的数字。但对于本题中，数组存在重复的数字，对于重复的数字也就意味着将两个相同的数字放到同一个位置是等效的，那么我们在为某一位选择数字时应该跳过重复的数字。

那么如何跳过？对于整个数组进行排序即可。同时，与[全排列](https://github.com/DanielXuuuuu/LeetCode/tree/master/46. Permutations)不同是，由于此时数组进行了排序，我们不能打乱数组的顺序，因此通过把选定的元素加入到新数组的方式，随之而来的是`first`变量也失效了，我们需要在每次循环中遍历整个原始数组。同时，为了标记已使用过的数字，引入了`used`数组进行记录。

整体而言，就是首先进行排序，然后思路是一样的，只是要处理因为排序带来的一些改变。

```c++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        vector<int> v;
        vector<bool> used(nums.size(), false);

        permute(res, nums, v, used);

        return res;
    }

    void permute(vector<vector<int>>& res, vector<int>& nums, vector<int>& v, vector<bool> used){
        if(v.size() == nums.size()){
            res.push_back(v);
            return;
        }
        for(int i = 0; i < nums.size(); i++){
            if(!used[i]){
                // 单纯判断重复会导致漏掉的情况，因此重复的前一个数必须还要是当前未被排序的
                if(i > 0 && nums[i] == nums[i - 1] && !used[i - 1])
                    continue;
                used[i] = 1;
                v.push_back(nums[i]);
                permute(res, nums, v, used);
                v.pop_back();
                used[i] = 0;
            }
        }
    }
};
```



