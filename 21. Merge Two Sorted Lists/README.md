# 合并两个有序链表

### 题目

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

##### 示例

```/
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



### 解法

#### 解法一

新建一个空链表，然后逐一比较两个链表的头节点，把小的节点赋值给新链表

```c++
// 执行用时 :12 ms, 在所有 cpp 提交中击败了62.39%的用户
// 内存消耗 :8.9 MB, 在所有 cpp 提交中击败了84.09%的用户

class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* newList = new ListNode(1);
        ListNode* temp = newList;
        while(l1 && l2){
            if(l1->val >= l2->val){
                temp->next = new ListNode(l2->val);
                l2 = l2->next;
            }else{
                temp->next = new ListNode(l1->val);
                l1 = l1->next;
            }
            temp = temp->next;
        }
        if(l1){
            temp->next = l1;
        }

        if(l2){
            temp->next = l2;
        }

        return newList->next;
    }
};
```



#### 解法二

递归实现，无需新建链表，直接在原有的两个链表上进行指针操作

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == NULL){
            return l2;
        }else if(l2 == NULL){
            return l1;
        }

        if(l1->val <= l2->val){
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }else{
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```

