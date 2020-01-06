# 寻找子集（含重复数字）

### 题目

给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

```/
输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```



### 题解

#### 解法一：迭代法

使用和迭代法求子集一样的方法，但在过程中进行去重。

首先思考重复的子集是如何产生的，以题中的示例为例，初始时只有一个空集：

+ 第一次循环得到：[]，[1]
+ 第二次循环得到：[]，[1]，[2]，[1,2]
+ 第三次循环得到：[]，[1]，[2]，[1,2]，**~~[2]~~**，**~~[1,2]~~**，[2,2]，[1,2,2]

可以看到，有两项重复了。根本的原因就在于：循环中，重复的元素在插入之前产生的子集时，会导致重复；而加入到上一次新产生的子集中却不会。

那么我们要做的就是先排序，然后在循环过程中记录一下每次循环新增的那部分子集即可。

```c++
// 执行用时 :12 ms, 在所有 C++ 提交中击败了75.32%的用户
// 内存消耗 :9.2 MB, 在所有 C++ 提交中击败了81.14%的用户

class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> res;
        res.push_back({});
        sort(nums.begin(), nums.end());

        int left = 0, right = 1, len = 0;
        for(int i = 0; i < nums.size(); i++){
            if(i != 0 && nums[i] == nums[i - 1]){
                left = res.size() - len;
            }else{
                left = 0;
            }
            right = res.size();
            len = right - left;

            for(int j = left; j < right; j++){
                vector<int> temp = res[j];
                temp.push_back(nums[i]);
                res.push_back(temp);
            }
        }
        return res;
    }
};
```

#### 解法二：递归+回溯

待补充

