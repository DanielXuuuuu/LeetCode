# 全排列

### 题目

给定一个**没有重复**数字的序列，返回其所有可能的全排列。

**示例:**

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```



### 解法

递归 + 回溯

模仿人为进行全排列的思路，首先固定第一个元素，然后再固定第二个元素，以此类推，那么后面的步骤都可以看作一次子数组的全排列，因此可以使用递归实现。同时，每个位置上有多个可选元素，那么就需要用到回溯。

以下代码使用`first`变量标记递归过程中当前需要确定的元素的位置，当`first == n`时即完成了一种可能的排列。同时在`for`循环中，循环的起始位置为`first`，即保持之前已确定的元素，选择其后未确定的元素并通过`swap`函数换到对应位置，然后继续下一轮递归。递归结束后，再次调用`swap`进行回溯。

```c++
// 执行用时 :8 ms, 在所有 cpp 提交中击败了99.73%的用户
// 内存消耗 :9.2 MB, 在所有 cpp 提交中击败了80.92%的用户
  
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {  
        vector<vector<int>> res;
        backtrace(nums.size(), nums, res, 0);
        return res;
    }

    void backtrace(int n, vector<int>& nums, vector<vector<int>>& res, int first){
        if(first == n){
            res.push_back(nums);
            return;
        }
        for(int i = first; i < n; i++){
            swap(nums[i], nums[first]);
            backtrace(n, nums, res, first+1);
            swap(nums[i], nums[first]);
        }
    }
};
```



