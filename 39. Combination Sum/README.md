# 组合总和

### 题目

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

**示例 2:**

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```



### 题解

递归，每次选一个数，检查选了这个数之后的和。但由于不能包含重复组合，也就是像`[2,2,3]`、`[2,3,2]`、`[3,2,2]`这样的只能包含一个。

#### 解法一

排序去重...我自己想到的去重方式，速度相当之慢。

```c++
// 执行用时 :1312 ms, 在所有 C++ 提交中击败了5.14%的用户
// 内存消耗 :439.7 MB, 在所有 C++ 提交中击败了5.02%的用户

class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> temp;
        set<vector<int>> s;
        combination(candidates, target, res, temp, s);
        return res;
    }

    void combination(vector<int> candidates, int target, vector<vector<int>>& res, vector<int> temp, set<vector<int>>& s){
        if(target < 0){
            return;
        }
        if(target == 0){
            sort(temp.begin(), temp.end());
            if(s.find(temp) == s.end()){
                s.insert(temp);
                res.push_back(temp);
            }
            return;
        }
        
        for(auto num : candidates){
            temp.push_back(num);
            combination(candidates, target - num, res, temp, s);
            temp.pop_back();
        }
    }
};
```

#### 解法二

思考组合重复的原因，例如`candidates = [2,3,6,7], target = 7`的情况，当我们选定第一个数字2时，就已经包含了`[2,2,3]`这种情况，也就是此时包含了以2开头的数组的全部情况。那么循环到选择数字3时，如果又去重新考虑前面的2，就会造成重复。因此，在循环中增加一个变量指向当前选择的元素下标，使得后续递归只能考虑后面的元素即可解决重复问题。

同时还进行了以下优化：

+ 首先对于数组进行排序，不排序也是可以的，只要保证上文的情况。但是排序后可以更好的剪枝，因为排序后循环中当出现数字大于当前剩余数字和`target`时，就可以提前结束循环了；

+ 将递归中小于0的情况放在for循环判断，减少了函数的调用次数；
+ 将函数传参方式改为引用传递，可以**大幅**减少时间；

```c++
// 执行用时 :12 ms, 在所有 C++ 提交中击败了93.99%的用户
// 内存消耗 :9.5 MB, 在所有 C++ 提交中击败了90.30%的用户

class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> temp;
        std::sort(candidates.begin(), candidates.end());
        combination(candidates, target, res, temp, 0);
        return res;
    }

    void combination(vector<int>& candidates, int target, vector<vector<int>>& res, vector<int>& temp, int cur){
        if(target == 0){
            res.push_back(temp);
            return;
        }
        
        for(int i = cur; i < candidates.size() && target - candidates[i] >= 0; i++){
            temp.push_back(candidates[i]);
            combination(candidates, target - candidates[i], res, temp, i);
            temp.pop_back();
        }
    }
};
```

