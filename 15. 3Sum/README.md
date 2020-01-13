# 三数之和

### 题目

给定一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

```
例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```



### 题解

#### 解法一

枚举所有3个数的组合，超时。



#### 解法二

双指针。

模仿两数和的解法，只不过此时`a+b=-c`。做法是：

+ 首先按照升序排列
+ 从头开始遍历，选择一个c，然后从c的右边寻找两个数，这两个数的和等于-c。为什么只需要在右边找就好了？因为如果在左边找，就又相当于右边两个数的和等于最左边的数的相反数，而这在之前已经计算过了。
+ 寻找过程中当c大于0时就可以结束了，因为c右边的两个数必定大于c，此时和不可能是0了。同时需要注意去重。

```c++
// 执行用时 :80 ms, 在所有 C++ 提交中击败了100.00%的用户
// 内存消耗 :18.6 MB, 在所有 C++ 提交中击败了24.46%的用户

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size() < 3){
            return {};
        }

        sort(nums.begin(), nums.end());
        vector<vector<int>> res;

        for(int i = 0; i < nums.size() - 2; i++){
            if(nums[i] > 0)
                break;
            if(i > 0 && nums[i] == nums[i - 1])
                continue;
            
            int sum = nums[i];
            int left = i + 1, right = nums.size() - 1;

            while(left < right)
                if(nums[left] + nums[right] == -sum){
                    vector<int> temp;
                    temp.push_back(sum);
                    temp.push_back(nums[left]);
                    temp.push_back(nums[right]);
                    res.push_back(temp);
                    while(++left < right && nums[left] == temp[1]);
                    while(--right > left && nums[right] == temp[2]);
                }else if(nums[left] + nums[right] < -sum){
                    left++;
                }else{
                    right--;
                }
        }
        return res;
    }
};
```

