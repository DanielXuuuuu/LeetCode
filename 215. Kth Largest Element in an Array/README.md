# 寻找数组中第k个最大元素

### 简单排序

这里使用了选择排序，每次选当前未排序的树中的最大元素，当选到第k个时即为结果

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        for(int i = 0; i < k; i++){
            int max = nums[i], maxIndex = i;
            for(int j = i + 1; j < nums.size(); j++){
                if(nums[j] > max){
                    max = nums[j];
                    maxIndex = j;
                }
            }
            swap(nums[i], nums[maxIndex]);
        }
        return nums[k - 1];
    }
};
```

### 快速排序思想，进行数组划分

每次选择数组其中一个元素，并把数组中所有大于这个元素的划分到左边，小于这个元素的划分到右边。若此时选择的这个基准元素恰好在k这个位置，即为所求，否则就递归地对于所要求的k位置所在的那一边重复进行划分。

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        return partition(nums, 0, nums.size() - 1, k);
    }

    int partition(vector<int>& nums, int begin, int end, int k){
        if(end == begin && begin == k)
            return nums[begin];

        int pivot = nums[begin], last_big = begin;
        for(int i = begin + 1; i <= end; i++){
            if(nums[i] > pivot){
                last_big++;
                swap(nums[i], nums[last_big]);
            }
        }
        swap(nums[begin], nums[last_big]);
        if(last_big == k - 1){
            return nums[last_big];
        }else if(last_big < k - 1){
            return partition(nums, last_big + 1, end, k);
        }else{
            return partition(nums, begin, last_big - 1, k);
        }
    }
};
```

### 最小堆

待实现..