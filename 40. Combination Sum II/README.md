# 组合总和 II

### 题目

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```



### 解法

题目要求找出和等于target的组合，且每个数字在一个组合中只能使用一次，同时组合不能重复。对应不同的需求，解题的思路如下：

+ 找出和等于target的组合：想到使用递归+回溯的思路，递归终止的条件是target ≦ 0；
+ 要求每个数字在每个组合中只能使用一次：想到使用一个额外的数组表示数字的使用情况。但在后续的过程中，改为了通过传递一个循环初始值`start`，使得下一层递归选数的范围是在上一层选择的数的后面，这样就保证了肯定不会选择之前选过的数了；
+ 要求组合不能重复：考虑组合重复的一种情况。如题目中的例子`[10，1，2，7，6，1，5]`，按照我们上面的方法就能容易得到`[1,2,5]`和`[2,1,5]`这样的重复组合。原因就是在于出现了重复的1，那么我们通过对原始数组排序就可以解决这个问题。再来看排序后的数组，变成了`[1,1,2,5,6,7,10]`，此时是肯定不会出现`[2,1,5]`这种情况了，但是却会出现多个`[1,2,5]`。那么，我们就需要在下一次选择数的时候，跳过重复的数。但注意循环的最开始一次不能跳，因为需要考虑到`[1,1,6]`的情况。

啰里八嗦说了一堆，感觉还是总结的有点乱 (￣▽￣)

```c++
// 执行用时 :8 ms, 在所有 C++ 提交中击败了92.43%的用户
// 内存消耗 :13 MB, 在所有 C++ 提交中击败了45.36%的用户

class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> temp;
        sort(candidates.begin(), candidates.end());
        helper(candidates, res, temp, target, 0);
        return res;
    }

    void helper(vector<int>& candidates, vector<vector<int>>& res, vector<int>& temp, int target, int start){
        if(target < 0)
            return;
        if(target == 0){
            res.push_back(temp);
            return;
        }
        for(int i = start; i < candidates.size(); i++){
            while(i > start && i < candidates.size()){
                if(candidates[i] == candidates[i - 1])
                    i++;
                else break;
            }
            if(i < candidates.size()){
                temp.push_back(candidates[i]);
                helper(candidates, res, temp, target - candidates[i], i + 1);
                temp.pop_back();
            }
        }
    }
};
```

