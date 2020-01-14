# 最接近的三数之和

### 题目

给定一个包括 *n* 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```



### 题解

#### 解法一

自己乱写的竟然一遍就过了..

简单说一下思路：和上一题三数之和之和类似，上一题要使得三数之和等于0，而本题要使得最接近一个数，即类似`a+b+c = target`的问题。也使用双指针实现，首先确定一个数`c`，然后让两个指针在其已排序好的右边从两端开始移动，要使得这两个数的和最接近`target - c`。比较两个指针指向的数之和与目标的大小，如果大于它就向左移动右指针，反之移动左指针，总之就是希望越接近目标越好。过程中要记录最小的差值。

```c++
// 执行用时 :8 ms, 在所有 C++ 提交中击败了90.20%的用户
// 内存消耗 :9 MB, 在所有 C++ 提交中击败了71.50%的用户

class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if(nums.size() == 3){
            return nums[0] + nums[1] + nums[2];
        }

        sort(nums.begin(), nums.end());
        int m = INT_MAX, res;
        bool find = false;

        for(int i = 0; i < nums.size() - 2; i++){
            // a + b + c = target;
            int sum = target - nums[i];
            int left = i + 1, right = nums.size() - 1;
            while(left < right){
                int temp = abs(sum - nums[right] - nums[left]);
                if(temp < m){
                    m = temp;
                    res = nums[i] + nums[left] + nums[right];
                }
                if(nums[left] + nums[right] == sum){
                    res = target;
                    find = true;
                    break;
                }else if(nums[left] + nums[right] > sum){
                    while(--right > left && nums[right] == nums[right + 1]);
                }else{
                    while(++left < right && nums[left] == nums[left - 1]);
                }
                
            }
            if(find){
                break;
            }
        }
        return res;   
    }
};
```

