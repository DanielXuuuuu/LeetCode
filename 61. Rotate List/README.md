# 旋转链表

### 题目

给定一个链表，旋转链表，将链表每个节点向右移动 *k* 个位置，其中 *k* 是非负数。

**示例 1:**

```
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```

**示例 2:**

```
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
```



### 题解

#### 解法一

找到要交换的点，分割链表后重新连接即可

```c++
// 执行用时 :8 ms, 在所有 C++ 提交中击败了94.15%的用户
// 内存消耗 :9.1 MB, 在所有 C++ 提交中击败了74.92%的用户

class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == NULL){
            return head;
        }

        ListNode* temp = head;
        int listLen = 0;
        while(temp != NULL){
            listLen++;
            temp = temp->next;
        }

        // 当k为listLen的整数倍时相当于没有移动
        k %= listLen;

        if(k == 0){
            return head;
        }

        // 找到倒数k+1个节点   
        ListNode* quick = head, *slow = head;
        while(quick->next != NULL){
            quick = quick->next;
            if(k != 0){
                k--;
            }else{
                slow = slow->next;
            }
        }

        ListNode* newHead = slow->next;
        slow->next = NULL;
        quick->next = head;
        return newHead;
    }
};
```





