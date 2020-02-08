# 颜色分类

### 题目

给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

**注意:**
不能使用代码库中的排序函数来解决这道题。

**示例:**

```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

**进阶：**

- 一个直观的解决方案是使用计数排序的两趟扫描算法。
  首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
- 你能想出一个仅使用常数空间的一趟扫描算法吗？



### 题解

#### 解法一

直接排序

#### 解法二

两趟扫描

#### 解法三

荷兰国旗问题，三指针方法。

在一次遍历的过程中，遇到0就放到左边，遇到2就放到右边，遇到1不进行任何操作。

```javascript
// 执行用时 :64 ms, 在所有 JavaScript 提交中击败了77.92%的用户
// 内存消耗 :33.8 MB, 在所有 JavaScript 提交中击败了49.80%的用户

/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var sortColors = function(nums) {
    let left = 0, right = nums.length - 1, cur = 0;
    while(cur <= right){
        if(nums[cur] === 0){
            swap(nums, cur++, left++);
        }else if(nums[cur] === 2){
            swap(nums, cur, right--);
        }else{
            cur++;
        }
    }
};

var swap = function(nums, a, b){
    const temp = nums[a];
    nums[a] = nums[b];
    nums[b] = temp;
}
```



