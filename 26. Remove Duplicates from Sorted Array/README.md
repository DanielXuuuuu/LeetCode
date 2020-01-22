# 删除排序数组中的重复项

### 题目

给定一个排序数组，你需要在**[原地](http://baike.baidu.com/item/原地算法)**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**并在使用 O(1) 额外空间的条件下完成。

**示例 1:**

```
给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
```

**示例 2:**

```
给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。
```



### 题解

```javascript
// 执行用时 :68 ms, 在所有 JavaScript 提交中击败了99.37%的用户
// 内存消耗 :36.7 MB, 在所有 JavaScript 提交中击败了67.57%的用户

/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    if(nums.length === 0){
        return 0;
    }

    let res = 1, duplicateNum = nums[0];
    for(let i = 1; i < nums.length; i++){
        if(nums[i] !== duplicateNum){
            duplicateNum = nums[i];
            nums[res++] = nums[i];
        }
    }

    return res;
};
```

